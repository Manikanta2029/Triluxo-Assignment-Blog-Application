# Triluxo-Assignment-Blog-Application
//LoginForm.js
import React, { useState } from 'react';
import axios from 'axios';

const LoginForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errorMessage, setErrorMessage] = useState('');

  const handleLogin = async () => {
    try {
      // Make an API call to your backend to authenticate the user
      const response = await axios.post('/api/login', {
        email,
        password,
      });

      // If login is successful, you can redirect the user or update the UI
      console.log('Logged in:', response.data);

      // Reset form fields
      setEmail('');
      setPassword('');
      setErrorMessage('');
    } catch (error) {
      // Handle login error
      setErrorMessage('Invalid email or password');
    }
  };

  return (
    <div>
      <h2>Login</h2>
      <form>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Email"
        />
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          placeholder="Password"
        />
        <button type="button" onClick={handleLogin}>
          Login
        </button>
      </form>
      {errorMessage && <p className="error">{errorMessage}</p>}
    </div>
  );
};
//CreateBlogForm
export default LoginForm;
import React, { useState } from 'react';
import axios from 'axios';
import ReactQuill from 'react-quill';
import 'react-quill/dist/quill.snow.css';

const CreateBlogForm = () => {
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');
  const [errorMessage, setErrorMessage] = useState('');
  const [successMessage, setSuccessMessage] = useState('');

  const handleCreateBlog = async () => {
    try {
      // Make an API call to your backend to create and save the blog
      const response = await axios.post('/api/create-blog', {
        title,
        content,
      });

      // If blog creation is successful, update the UI
      setSuccessMessage('Blog created successfully');

      // Reset form fields
      setTitle('');
      setContent('');
      setErrorMessage('');
    } catch (error) {
      // Handle blog creation error
      setErrorMessage('Error creating the blog');
    }
  };

  return (
    <div>
      <h2>Create and Publish Blogs</h2>
      <form>
        <input
          type="text"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          placeholder="Title"
        />
        <ReactQuill
          value={content}
          onChange={setContent}
          placeholder="Write your blog content..."
        />
        <button type="button" onClick={handleCreateBlog}>
          Publish
        </button>
      </form>
      {errorMessage && <p className="error">{errorMessage}</p>}
      {successMessage && <p className="success">{successMessage}</p>}
    </div>
  );
};
//CommentSection
export default CreateBlogForm;
import React, { useState } from 'react';
import axios from 'axios';

const CommentSection = ({ blogId }) => {
  const [comment, setComment] = useState('');
  const [comments, setComments] = useState([]);
  const [errorMessage, setErrorMessage] = useState('');

  const handlePostComment = async () => {
    try {
      // Make an API call to your backend to post a comment
      const response = await axios.post(`/api/post-comment/${blogId}`, {
        comment,
      });

      // If posting comment is successful, update the UI
      setComments([...comments, response.data.comment]);

      // Reset comment field and error message
      setComment('');
      setErrorMessage('');
    } catch (error) {
      // Handle comment posting error
      setErrorMessage('Error posting the comment');
    }
  };

  return (
    <div>
      <h2>Comment on this Blog</h2>
      <div className="comment-list">
        {comments.map((comment, index) => (
          <div key={index} className="comment">
            {comment}
          </div>
        ))}
      </div>
      <input
        type="text"
        value={comment}
        onChange={(e) => setComment(e.target.value)}
        placeholder="Write your comment..."
      />
      <button type="button" onClick={handlePostComment}>
        Post Comment
      </button>
      {errorMessage && <p className="error">{errorMessage}</p>}
    </div>
  );
};

export default CommentSection;
