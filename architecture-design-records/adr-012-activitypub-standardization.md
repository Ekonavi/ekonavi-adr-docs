# ADR 012: Integration of ActivityPub Standard

### Changelog
* 2023-08-21: Initial draft

### Status
DRAFT Not Implemented

### Abstract
This ADR proposes the integration of the ActivityPub standard to introduce social networking functionalities including user profiles, messaging, public forum discussions, media attachments, and various other features within the existing system.

EKONAVI would not initially enable cross-server / cross-site posting, but when enabled, users would have the option to post just on EKONAVI or to all connected servers and services. Adopting this standard early provides:

1) A WC3 approved standard for managing social network interactions
2) Future option to federate EKONAVI network
3) Standardization of regeration activity data

### Context
With the desire to enhance user interaction and establish a federated communication system, integrating ActivityPub aligns with these objectives. It provides a common protocol to cover specific functionalities within the platform:

* Attaching Images/Videos to Posts
* User Forum with Topics and Hashtags
* FAQ System
* Library with Ebooks and Videos
* Reservations/Booking/Events System
* Marketplace

## What is ActivityPub?
ActivityPub is a protocol for decentralized social networking, allowing users to interact across different servers. It's a W3C Recommendation, meaning it has been endorsed by the World Wide Web Consortium.

### How is it Used?
ActivityPub works by defining a server-to-server protocol, allowing users on different servers to follow each other, send messages, and perform other social interactions. The Client-to-Server protocol helps the user's application communicate with their own server.

### Integration with Nextjs / Hasura / Vercel / CloudFlare Workers API
#### User Profiles:
* ActivityPub can be used to create decentralized user profiles. Profiles would include the user's preferred username, display name, profile picture, etc.

#### Messaging:
* With ActivityPub, users can send messages to each other, even if they're on different servers.

#### Public Forums and Hashtags:
* Users can create posts on public forums that include hashtags. These posts can be federated across various servers.

### What is the Fediverse or Federated?
"Fediverse" is a combination of "federated" and "universe." In this context, it refers to a system where servers can communicate with one another, allowing users on different servers to interact. "Federated" refers to the network of interconnected but independent servers.


## Understanding ActivityPub: A Toy Box Analogy
Imagine ActivityPub is like a big toy box where you can keep different toys and play with your friends. Let's talk about the main "toys" (types) you have in this box.

#### People (Actors):
These are like the little action figures. They represent you and your friends and can do things like talk, share, and play with other toys.

#### Activities:
These are like the games your action figures can play. Examples include sharing a picture (posting) or making a new friend (following).

#### Objects:
Think of these as the things the action figures can play with. They can be pictures, messages, videos, or even fun stories.

#### Collections:
This is like having little toy bins within your toy box. You can group toys together in these bins. For example, you might have a bin for all your toy cars and another one for your building blocks.

### Main Types in ActivityPub
Now, let's look at these toys with a grown-up lens and see what they really are:

#### Actors:
These represent users in ActivityPub. There are different types like "Person" (regular user), "Organization" (a group or company), and "Service" (automated account).

#### Activities:
These are the actions that actors can do. Some main examples are:
* Create: Making a new post or object.
* Follow: Becoming friends with another actor.
* Like: Showing that you enjoy a particular post.

#### Objects:
These are the things actors create and interact with. Examples are:
* Note: A short message or post.
* Image: A picture.
* Video: A movie or clip.
* Article: A longer piece of writing.

#### Collections:
These are groups of objects and actors. Examples are:
* OrderedCollection: A group with a specific order, like a list of your best friends.
* Collection: A general group, like a toy bin with mixed toys.

### Playing with ActivityPub
So, when you're using a website with ActivityPub, you're like playing with these toys. You can make new friends, share pictures and stories, and put them into groups. You can even play with friends who have different toy boxes if those toy boxes also use ActivityPub!

And just like playing with toys, it should be fun, creative, and sometimes a bit tricky, but always something you can learn and enjoy!

## Media Handling: Attaching Images/Videos
Integration with CloudFlare allows handling of image and video uploads. ActivityPub objects can be used to represent these media files. Using ActivityPub with CloudFlare for hosting media, you can include media attachments within the "object" of an ActivityPub activity. Here's an example of handling an image upload:

```typescript
const handleImageUpload = async (req: NextApiRequest, res: NextApiResponse) => {
  // Handle image upload to CloudFlare
  // Store the image URL in your database
  const imageUrl = 'your_image_url_from_cloudflare';

  // Create the ActivityPub object with the image attachment
  const activity = {
    '@context': 'https://www.w3.org/ns/activitystreams',
    type: 'Create',
    object: {
      type: 'Image',
      url: imageUrl,
      // Other attributes like caption, etc.
    },
    // Other activity attributes
  };

  // Handle the activity, e.g., save to database, send to followers, etc.
};

export default handleImageUpload;

```

Videos would be handled in a similar manner.

## Applying to specific EKONAVI features:

### User Forum with Topics and Hashtags
ActivityPub can be used to create a federated forum with topics and hashtags.

#### Topics:
* Represented as collections or objects within ActivityPub.

#### Hashtags:
* Represented as tags within posts and can be used to categorize and filter posts.

### FAQ System
ActivityPub can represent FAQs as individual objects, organized into collections.

#### Questions and Answers:
* Each Q&A can be an object with properties for the question and answer.

#### Categories:
* Different categories of FAQs can be represented as collections.

### Library with Ebooks and Videos
A library system could use ActivityPub to manage access to ebooks and videos.

#### Ebooks and Videos:
* Represented as objects with metadata and access URLs.

#### Collections:
* Grouped into collections based on genre, author, etc.

#### Access Control:
* Use ActivityPub's following/followers mechanisms to manage access.

### Reservations/Booking/Events System
ActivityPub can be used to manage reservations, bookings, and events.

#### Events:
* Represented as objects with details such as date, location, etc.

#### Reservations/Bookings:
* Handled via custom activities like 'Reserve', 'Book', etc.

#### Notifications:
* Notify users of booking confirmations, changes, etc.

### Marketplace
An online marketplace can be implemented using ActivityPub.

#### Products:
* Represented as objects with details like price, description, etc.

#### Vendors:
* Represented as actors within the ActivityPub system.

#### Purchasing:
* Handled via custom activities like 'Order', 'Purchase', etc.

### Summary
Integrating ActivityPub within your site allows for a consistent, standard way to handle various social features, from media attachments to more complex use cases like forums, libraries, booking systems, and marketplaces. It enables structured data representation, flexible interactions, and potential future federation with other ActivityPub-compliant systems. Careful design and integration are required to fit your specific needs and architecture.


## Consequences
#### Positive
* Standardized implementation.
* Flexibility and federation potential.
* Rich interactions.

#### Negative
* Complexity in integration.
* Maintenance and compliance.

#### Neutral
* Adaptation to specific needs.

### Further Discussions
* Scaling considerations.
* Security measures.
* Ongoing alignment with the evolving standard.

## References
* [ActivityPub Specification](https://www.w3.org/TR/activitypub/)
* [GitHub Repository for ActivityPub](https://github.com/w3c/activitypub)
* [Activity.next, ActivityPub server with Next.JS](https://github.com/llun/activities.next)
