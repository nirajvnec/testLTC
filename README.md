{apiErrorsOverlayOpen && (
  <div style={{ backgroundColor: 'lightcoral', padding: '10px', margin: '10px 0' }}>
    {apiErrors.map((msg, index) => (
      <div key={index}>{msg}</div>
    ))}
  </div>
)}