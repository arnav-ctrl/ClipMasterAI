import React, { useState } from 'react';
import axios from 'axios';

function App() {
  const [videoLink, setVideoLink] = useState('');
  const [language, setLanguage] = useState('en');
  const [loading, setLoading] = useState(false);
  const [fileUrl, setFileUrl] = useState('');

  const handleSubmit = async () => {
    setLoading(true);
    try {
      const response = await axios.post('http://localhost:5000/process_video', {
        youtube_link: videoLink,
        language
      });
      setFileUrl(response.data.file);
      setLoading(false);
    } catch (error) {
      console.error('Error processing video:', error);
      setLoading(false);
    }
  };

  return (
    <div className="App">
      <h1>AI Video Clip Generator</h1>
      <input
        type="text"
        value={videoLink}
        onChange={(e) => setVideoLink(e.target.value)}
        placeholder="Enter YouTube Video Link"
      />
      <div>
        <label>Language:</label>
        <select value={language} onChange={(e) => setLanguage(e.target.value)}>
          <option value="en">English</option>
          <option value="hi">Hindi</option>
        </select>
      </div>
      <button onClick={handleSubmit}>Generate Video</button>
      {loading && <p>Processing...</p>}
      {fileUrl && (
        <div>
          <p>Video Ready! Download from:</p>
          <a href={fileUrl} download>
            Download Video
          </a>
        </div>
      )}
    </div>
  );
}

export default App;
