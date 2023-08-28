# ADR 015: Video Implementation & Multi-Media Upload Widget

## Changelog
* 2023-08-28: Initial draft

## Status
DRAFT Not Implemented

## Abstract
This ADR proposes a strategy for implementing a multi-media upload widget that allows for dynamic creation of new items, supports both video and image uploads, and provides real-time upload progress. The proposed implementation will use a Global React Component for uploads, and each media item will be stored as a separate row in a Hasura PostgreSQL database with an added sort column for user-defined order. For tracking upload progress and handling uploads, we will use React-Hook-Form along with CloudFlare Stream Video for video uploads. The focus is on direct client uploads, eliminating the need for worker involvement.

## Context

### Requirements
1. The upload widget should allow the dynamic creation of new items.
2. Support for both video and image uploads.
3. Uploads should show progress.
4. Uploaded media should be sortable.
5. Each media item should have a description.

### Tech Stack
- Next.js
- TypeScript
- React-Hook-Form
- Hasura PostgreSQL
- CloudFlare Stream Video

## Benchmarking

### Service Worker vs Global Component for Uploads
The service worker approach was considered but discarded due to added complexity and partial browser support. The Global React Component approach was chosen for its simplicity and full browser support.

### Direct Client Upload vs Worker Upload
Direct client upload was chosen to handle all media uploads, both images and videos, in the same manner. This is mainly to track upload progress consistently across all media types.

### Data Storage: Individual Rows vs JSONB Array
Two approaches were considered for storing media items in the Hasura PostgreSQL database: storing each media item as an individual row or storing all media items related to a particular entity in a single JSONB array column. 

#### Individual Rows

##### Pros
1. **Flexibility:** Each media item can have its own set of metadata and can be queried or modified individually.
2. **Better Support for Sorting:** A separate 'sort' column can be added to maintain the user-defined order of media items.
3. **Atomic Operations:** Easier to handle transactional integrity when each media item is a separate row.

##### Cons
1. **Increased Number of Rows:** May lead to more rows in the database, which could impact query performance if not managed properly.

#### JSONB Array

##### Pros
1. **Simplicity:** Easier to retrieve all media items related to an entity in one query.
2. **Reduced Number of Rows:** Lessens the number of rows in the database.

##### Cons
1. **Complex Querying:** More complex queries are required to manipulate or retrieve individual media items.
2. **Atomicity:** Harder to maintain transactional integrity when modifying individual elements within the JSONB array.

After evaluating the pros and cons, the decision was made to use individual rows for storing each media item, mainly due to the flexibility it offers and its compatibility with sorting and transactional integrity.

## Upload Implementation Comparison: Service Worker vs Global React Component for Uploads

### Service Worker Approach

#### Pros:

1. **Background Processing:** Service Workers can handle uploads even if the user navigates away from the page or closes it.
2. **Separation of Concerns:** Business logic for uploading can be separated from the UI, allowing for a cleaner codebase.
3. **Resilience:** Offers the ability to retry failed uploads automatically.

#### Cons:

1. **Browser Support:** Not all browsers fully support Service Workers.
2. **Complexity:** Implementing a Service Worker adds another layer of complexity to your application.
3. **Debugging:** Debugging Service Workers can be challenging due to their lifecycle and scope.

### Global React Component Approach

#### Pros:

1. **Simplicity:** Easier to implement, with better browser support.
2. **Uniformity:** A single method for uploading both video and image files simplifies code maintenance.
3. **Immediate Feedback:** Can easily update UI components in real-time to show progress or errors.

#### Cons:

1. **Blocking:** Can block the main thread if not implemented carefully, which could lead to a poor user experience.
2. **State Management:** More front-end logic needed to manage the state of each media element, including errors and retries.
3. **No Background Processing:** Uploads will be interrupted if the user navigates away from the page.


## UI Interface Implementation: Considerations and Suggestions

### Approach 1: Media Uploader with Sorting and Progress Bar

#### Pros:
1. **Immediate Feedback:** Users see real-time progress.
2. **Better UX:** Sorting and media descriptions improve user engagement.
3. **Faster Overall Submission:** Videos and images upload while the user is filling other parts of the form.

#### Cons:
1. **Complexity:** Requires more front-end logic to manage the state of each media element, including errors and retries.
2. **Database Consistency:** You'll need to handle cases where a media upload fails after the parent object has been created.

### Approach 2: Standard Multi-Field Form with Progress Indication

#### Pros:
1. **Simpler Implementation:** Easier to build.
2. **Database Consistency:** All media elements upload together before creating the parent object.

#### Cons:
1. **Slower Submission:** User has to wait for all uploads before submitting the form.
2. **No Real-Time Feedback:** Lack of individual progress bars for each media element.

### Final Decision

After evaluating the pros and cons, we chose to go with a **Media Uploader with Sorting and Progress Bar** for a better UX even though more complex. Ultimately, we should delight our users even if it takes our devs twice as long to implement.

## Suggested Code Structure

Given the potential risks and limitations of generating a UUID on the client-side, such as collision risks and security concerns, we recommend creating a stub post first on the server. The UUID for the object will be generated and returned from the database, to be populated in the form as the user fills it out.

### Stub Post and Media Elements in Context of UI Approaches

#### Approach 1: Media Uploader with Sorting and Progress Bar

1. **Create Stub Post**: On client initialization, send a request to create a stub post. The server generates a UUID, stores it in the database, and returns it to the client.
2. **Individual Media Uploads**: As each media file is selected or dragged into the media uploader, immediately upload it to Cloudflare's video streaming service using the returned UUID as the parent object identifier.
3. **Progress Bar**: Show a progress bar next to each media element to indicate its upload status.
4. **Sorting**: Allow the user to sort media elements while they're being uploaded.

#### Approach 2: Standard Multi-Field Form with Progress Indication

1. **Create Stub Post**: Once the user starts filling out the form, send a request to create a stub post. The server generates a UUID, stores it in the database, and returns it to the client.
2. **Individual Media Uploads**: As the user attaches media in the form, queue them for upload. Once the user submits the form, start the upload process.
3. **Progress Indication**: Use a circular progress indicator at the bottom of the form to show overall upload progress.
4. **No Sorting**: This approach does not allow sorting of media while they're being uploaded but offers a simpler interface.

### Final Thoughts

We strongly recommend creating a stub post for the reasons mentioned above. Approach 1 is optimal for use-cases requiring advanced functionalities like real-time sorting and progress monitoring. Approach 2 is best suited for simpler forms where advanced media manipulation is not required.

This strategy aligns server-side UUID generation and individual media uploads within the UI implementations, ensuring a coherent and efficient upload process.


## Packages to be Used

### React-Hook-Form
- For form validation and management
- Makes it easier to implement dynamic form fields for media uploads

### React-DnD (Drag and Drop)
- For implementing the sorting functionality in the media uploader.
- Provides a better UX for rearranging the uploaded media.

### react-uploady or react-dropzone
- For handling file uploads with progress.
- Provides built-in support for progress indication.

### CloudFlare API
- For generating one-time upload links and for direct uploads to CloudFlare storage.

## Uploader UI Sudocode

```bash
npm install react-dropzone react-sortable-hoc react-hook-form @react95/progress-bar uuid
```

```typescript
import React, { useState } from 'react';
import { useFieldArray, useForm } from 'react-hook-form';
import { SortableContainer, SortableElement, SortableHandle } from 'react-sortable-hoc';
import Dropzone from 'react-dropzone';
import ProgressBar from '@react95/progress-bar';
import { v4 as uuidv4 } from 'uuid';

interface MediaUploadField {
  id: string;
  file: File | null;
  description: string;
  uploadProgress: number;
  // Add more fields if necessary
}

const DragHandle = SortableHandle(() => <span>::</span>);

const SortableItem = SortableElement(({ field }) => (
  <div>
    <DragHandle />
    <Dropzone onDrop={acceptedFiles => /* upload logic here */}>
      {({ getRootProps, getInputProps }) => (
        <div {...getRootProps()}>
          <input {...getInputProps()} />
          <p>Drag 'n' drop some files here, or click to select files</p>
        </div>
      )}
    </Dropzone>
    <input name={`media[${field.id}].description`} placeholder="Description" />
    <ProgressBar percent={field.uploadProgress} />
  </div>
));

const SortableList = SortableContainer(({ fields }) => {
  return (
    <div>
      {fields.map((field, index) => (
        <SortableItem key={field.id} index={index} field={field} />
      ))}
    </div>
  );
});

const MediaUploader: React.FC = () => {
  const { register, control, handleSubmit } = useForm<{ media: MediaUploadField[] }>();
  const { fields, append, remove, move } = useFieldArray({
    control,
    name: 'media'
  });

  const onSubmit = (data) => {
    // Handle the submit logic
  };

  const handleSortEnd = ({ oldIndex, newIndex }) => {
    move(oldIndex, newIndex);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <SortableList fields={fields} onSortEnd={handleSortEnd} useDragHandle={true} />
      <button type="button" onClick={() => append({ id: uuidv4(), file: null, description: '', uploadProgress: 0 })}>
        Add Media
      </button>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MediaUploader;
```

## Global Component to Handle Uploading State

### Global Uploader Component in Component Tree
The Global Uploader Component should be positioned high in the component tree, near the application's root, to ensure it can be easily accessed by various parts of the application that might require media uploading functionalities.

### Tracking Multiple Uploads and XHR Progress State
The Global Uploader Component will manage multiple uploads through an internal state object where each upload is assigned a unique identifier (UUID). This identifier will be used to track and update the progress of each upload through XMLHttpRequest (XHR) events.

### Risk of Unmounting the Component
If the user closes the page or navigates away, the uploads would be interrupted. To mitigate this, an "Are you sure you want to leave? Uploads are in progress." popup alert will be shown when the user attempts to navigate away from the page.

### Suggested Code Structure and Extensive Pseudocode

```ts

import React, { useState, useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { v4 as uuidv4 } from 'uuid';

// Define your interface
interface UploadItem {
  id: string;
  file: File | null;
  progress: number;
}

const GlobalUploader: React.FC = () => {
  // State to track multiple uploads
  const [uploads, setUploads] = useState<Record<string, UploadItem>>({});

  // Hook form for other functionalities
  const { register, handleSubmit } = useForm();

  // Handle form submit
  const onSubmit = (data: any) => {
    // Implement submit logic
  };

  useEffect(() => {
    // Listen for the beforeunload event to warn the user
    const handleBeforeUnload = (event: BeforeUnloadEvent) => {
      if (Object.values(uploads).some(upload => upload.progress < 100)) {
        event.preventDefault();
        event.returnValue = "Are you sure you want to leave? Uploads are in progress.";
      }
    };

    window.addEventListener('beforeunload', handleBeforeUnload);
    return () => {
      window.removeEventListener('beforeunload', handleBeforeUnload);
    };
  }, [uploads]);

  // Trigger the upload
  const triggerUpload = (file: File) => {
    const uploadId = uuidv4(); // Unique identifier for the upload
    
    const xhr = new XMLHttpRequest();
    xhr.upload.onprogress = (event) => {
      const progress = (event.loaded / event.total) * 100;

      setUploads(prevUploads => ({
        ...prevUploads,
        [uploadId]: { id: uploadId, file, progress }
      }));
    };

    // Complete and error handling here
    xhr.onload = () => {
      // Complete logic
    };
    xhr.onerror = () => {
      // Error handling
    };

    xhr.open('POST', 'YOUR_UPLOAD_ENDPOINT_HERE', true);
    xhr.send(file);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Render your form fields here */}
      <input type="file" onChange={e => e.target.files && triggerUpload(e.target.files[0])} />
      <button type="submit">Submit</button>
    </form>
  );
};

export default GlobalUploader;
```

## Resources

### Tech Stack and Services
1. **Next.js**: [Official Website](https://nextjs.org/)
2. **TypeScript**: [Official Website](https://www.typescriptlang.org/)
3. **React-Hook-Form**: [NPM Package](https://www.npmjs.com/package/react-hook-form)
4. **Hasura**: [Official Website](https://hasura.io/)
5. **PostgreSQL**: [Official Website](https://www.postgresql.org/)
6. **CloudFlare Stream Video**: [Official Website](https://www.cloudflare.com/products/stream/)
7. **CloudFlare Workers**: [Official Website](https://workers.cloudflare.com/)
8. **CloudFlare KV**: [Official Website](https://developers.cloudflare.com/workers/runtime-apis/kv)

### React Packages for File Uploads (for consideration)
1. **react-dropzone**: [NPM Package](https://www.npmjs.com/package/react-dropzone)
2. **react-uploady**: [NPM Package](https://www.npmjs.com/package/react-uploady)
3. **filepond-react**: [NPM Package](https://www.npmjs.com/package/filepond-react)

### Service Workers
1. **MDN Web Docs on Service Workers**: [MDN Guide](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)



