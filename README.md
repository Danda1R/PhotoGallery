Hello Division B! This photo gallery is an example of how to implement images into the Juego Juegos App.

I have put comments next to each function to explain its use. Please read below for the most relevant functions for the JJ app.

App.js

```
    postsArray = await Promise.all(
      postsArray.map(async (post) => {
        const imageKey = await Storage.get(post.image);
        post.image = imageKey;
        return post;
      })
    );
```

This function goes through all of the post in the app to display the photos.
The image key that was saved in the graphQL schema is used to locate the unique image URL in the S3 storage.

CreatePost.js

```
    <input type="file" onChange={onChangeFile} />
      {formState.file && (
        <img className={imageStyle} alt="preview" src={formState.file} />
      )}
```

This input will create a "Choose file" button for users as seen below.
When the photo is added, it will trigger the onChangeFile function

```
function onChangeFile(e) {
    e.persist();
    if (!e.target.files[0]) return;
    const fileExtPosition = e.target.files[0].name.search(/.png|.jpg|.gif/i);
    const firstHalf = e.target.files[0].name.slice(0, fileExtPosition);
    const secondHalf = e.target.files[0].name.slice(fileExtPosition);
    const fileName = firstHalf + "_" + uuid() + secondHalf;
    console.log(fileName);
    const image = { fileInfo: e.target.files[0], name: fileName };
    updateFormState((currentState) => ({
      ...currentState,
      file: URL.createObjectURL(e.target.files[0]),
      image,
    }));
  }
```

This function will add the image's uuid to the filename before the user inputs the post

```
await Storage.put(formState.image.name, formState.image.fileInfo);
```

This line will put the image in the storage with the modified file name and original name attached and saved.

Posts.js

```
        <Link to={`/post/${post.id}`} className={linkStyle} key={post.id}>
          <div key={post.id} className={postContainer}>
            <h1 className={postTitleStyle}>{post.name}</h1>
            <img alt="post" className={imageStyle} src={post.image} />
          </div>
        </Link>
```

This function uses the Link to and key parameters to get the correct post variable and the correct post name and image url

Post.js

```
      const currentPost = postData.data.getPost;
      const image = await Storage.get(currentPost.image);

      currentPost.image = image;
```

This function obtains the image from the S3 bucket using the post data image url from the GraphQL schema

These are the most important and unique functions to implement in the JJ app.

Please DM me if you have any additional questions.
