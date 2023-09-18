const formData = new FormData(); formData.append('file', file, file.name);

// Note: Normally, for multipart requests, you don't have to set 'Content-Type' header.
// The browser will set it automatically, including boundary information.
return this.http.post(`${this.apiUrl}/Tree/UploadZipFileAndFetchTreeData`, formData)
  .pipe(
    catchError(this.handleError)
  );
