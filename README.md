## Description

Our project, Libftea, is a social platform designed for fashion enthusiasts who want to share their style and connect with a like-minded community. Users can post outfit photos, interact with friends and other fashion lovers, and engage with content through likes and comments, creating an interactive and inspiring space for self-expression through clothing.

The goal of Libftea is to build a thriving fashion community where people can discover new styles, get inspired by others, and showcase their own creativity, all of that on our platform.

After signing up and logging in, users can access a news feed displaying outfits posted by other users. Each post can be liked and commented on, with interactions updated in real time to ensure a smooth and engaging experience.
Each user has a personal profile page that includes their profile picture, their published outfits, and all associated interactions (likes and comments). Users can also browse other profiles, add friends, view online status, as well as block or report other users when necessary.
The platform also features a tournament system based on themed challenges. Administrators can create competitions where users participate by submitting their outfits. The community can vote for a defined period, and a winner is automatically selected at the end of the tournament.
A notification system keeps users informed in real time about interactions related to their activity, such as likes, comments, or tournament results.
The entire application is designed to provide a smooth, interactive, and community-driven user experience.

## Live Demo (Frontend Showcase)

A public version of the project is available online in order to allow recruiters and reviewers to explore the interface and user experience of the platform without needing to install the project locally.

### Live demo:

https://opheliamarboeuf.github.io/libftea

This hosted version runs frontend-only and is intended as a visual showcase of the application.

For security and moderation reasons, the backend services are intentionally disabled in this environment. As a result:
- user registration and authentication are disabled

The goal of this demo is to provide a guided preview of the platform’s design, navigation, and main features.

After landing on the homepage, you will be redirected to a user selection page where you can choose from predefined accounts. This allows immediate access to the application interface and enables you to explore the different pages and UI components as they would appear in a real user session.

To experience the full functionality of the project (backend, database, real-time features, authentication, etc...), please run the application locally by following the installation instructions below.

## Technical Stack

The combination of React and NestJS with TypeScript on both ends was chosen to ensure consistently and reduce context-switching for the team. PostgreSQL paired with Prisma gave us a robust and developer-friendly data layer, while Docker ensured that the project ran identically on every machine regardless of local configuration.

#### Frontend

React (TypeScript):
Used to build a dynamic and component-based user interface. React allows efficient state management and reusable UI components, improving maintainability and scalability.

Tailwind CSS:
Chosen over traditional CSS to enable faster and more consistent styling. Its utility-first approach improves development speed, enforces design consistency, and reduces the need for custom CSS.

#### Backend

Selected for its modular architecture and strong structure inspired by enterprise frameworks, the backend was developed with NestJS.It provides scalability, maintainability, and clear separation of concerns. NestJS is widely used in professional environments, making it a relevant choice for real-world development. TypeScript was used across the backend as well, ensuring type safety throughout the entire codebase. Anthentication was implemented using JWT (JSON Web Tokens), providing a stateless and secure way to manage user sessions.

#### Database

We chose PostgreSQL as out database system for its reliability, support for complex relational data, and strong community support. It was a natural fit for our data model, which involves relationships between users, posts, and interactions. It was chosen for its reliability, performance, and widespread adoption in production environments.
Database access was managed through Prisma, an ORM that simplified query writing and schema management while keeping things type-safe and helps prevent common vulnerabilities such as SQL injections.

#### Infrastructure

The project was containerized using Docker, making it easy to set up consistent development environments across the team and simplifying future deployment.

## Features List

#### Landing & Authentication (Ophélia, Charlotte)

- Landing page presenting the platform, its concept, and key features
- User registration with email and password
- User login system
- GitHub OAuth authentication
- Two-Factor Authentication (2FA) via email code
- Form validation and error handling
- JWT-based authentication and protected routes

#### User Profile (Ophélia)

- Personal profile page displaying user information
- Avatar and cover image upload
- Bio editing
- Display of user's posts
- Access to profile settings

#### Posts (Ophélia)

- Create a post with image, title, and caption
- Edit and delete own posts
- Tag system for categorizing posts
- Global feed (timeline) displaying posts
- Ability to hide posts from specific users

#### Likes & Comments (Léonore)

- Like and unlike posts
- Comment on posts
- Reply to comments (nested comment system)
- Delete own comments

#### Friends (Léonore, Ophélia)

- Send, accept, and reject friend requests
- Block other users
- Friends list management
- View online status of friends

#### Chat (Ari)

- Private conversations between users
- Real-time messaging using WebSockets
- Persistent message history
- Invitation system to invite users to join tournaments via messaging

#### Tournament / Battle (Charlotte)

- Create tournaments with theme, description, rules, max players, and start/end dates (admin only)
- Plan tournaments in advance
- Join a tournament by submitting a post
- Tournament lifecycle management (UPCOMING → ACTIVE → FINISHED)
- Voting system with like-based ranking
- Automatic winner selection at the end of the tournament
- Winner highlight (“Last week’s winner”) displayed in the next tournament

#### Notifications (Léonore)

Real-time notifications for:

- likes
- comments and replies
- friend requests and acceptances
- new tournaments
- tournament results (win/end)
- role changes (promotion/demotion)
- Mark notifications as read

#### Internationalisation (i18n) (Léonore, Charlotte)

- Full UI translation in:
  - English
  - French
  - Japanese
- Language switcher available in the interface

#### Moderation & Admin (Ophélia)

- Role-based access control (USER, MOD, ADMIN)
- Report system for posts and users:
  - spam
  - harassment
  - inappropriate content
  - other

- Moderation dashboard for admins and moderators
- Ban and unban users
- Delete any post
- Promote and demote users (MOD / ADMIN)
- Moderation logs ensuring full action traceability

#### Legal (Léonore, Charlotte)

Privacy Policy page (EN / FR / JP)
Terms of Service page (EN / FR / JP)
Accessible links from login and registration pages

## Database Schema

#### Overview

The application is built on a relational database (PostgreSQL) structured around core social network entities such as users, posts, interactions, and tournaments.
The schema is designed to ensure:

- data integrity through strong relations and constraints
- scalability for social features (likes, comments, friendships, messaging)
- support for advanced modules such as moderation and tournaments

#### Core entities and relationships

#### User:

- Central entity of the application
- Stores authentication data, role (USER, ADMIN, MOD), and account status (banned)
- Relations: posts (one-to-many), comments (one-to-many), likes (one-to-many), friendships (self-relation), messages and conversations, notifications, reports and moderation logs

#### Post:

- Represents an outfit shared by a user
- Contains image, caption, title, timestamps
- Relations: belongs to one user (author), has many likes and comments, can participate in a tournament (BattleParticipant), can be reported or hidden

#### Comment:

- Linked to a post and a user
- Supports nested comments via a self-relation (replies system)

#### Like:

- Connects a user to a post
- Enforced constraint: one like per user per post (@@unique[userId, postId])

#### Friendship:

- Self-relation between users
- Includes status (PENDING, ACCEPTED, BLOCKED)
- Enforces uniqueness between two users

#### Conversation & Message:

- Messaging system between users
- A conversation contains multiple messages
- Supports real-time communication features

#### Battle:

- Represents a tournament
- Key fields: theme, startsAt, endsAt, status (UPCOMING, ACTIVE, FINISHED), winnerId (linked to User)

#### BattleParticipant:

- Junction table linking: a user, a post, a tournament
- Enforces:
  one participation per user per tournament (@@unique[battleId, userId])

#### Report:

- Allows users to report posts or other users
- Includes: report type (SPAM, HARASSMENT, etc.), status (PENDING, ACCEPTED, REJECTED)
- Linked to: reporter (User), target (User or Post), moderator handling the report

#### ModerationLog:

- Stores all moderation actions (ban, delete, role changes, etc.)
- Ensures traceability with: actor (admin/mod), target (user, post, or tournament)

#### Notification:

- Stores user notifications
- Includes: type (LIKE, COMMENT, BATTLE_WIN, etc.), read status,optional metadata (JSON for flexibility)

#### PostHiddenForUser / UserHiddenForUser:

- Allow users to hide posts or other users
- Implemented through junction tables with uniqueness constraints

#### Additional entities:

- Profile: stores user profile data (avatar, bio, cover)
- Tag / PostTag: tagging system for posts (many-to-many relationship)

#### Key design choices:

- Strong use of relations to model a real social network structure
- Junction tables (BattleParticipant, PostTag, Hidden entities) to handle many-to-many relationships
- Database constraints (@@unique) to enforce business rules (e.g. one like per post, one participation per tournament)
- Enums to standardize states (roles, reports, tournament lifecycle, notifications)
- Soft deletion patterns (deletedAt, bannedDeletion) to preserve data integrity while hiding content

## Individual Contributions

#### Fashion Tournament System

- Designed and implemented the full tournament system
- Tournament creation (admin only) with theme, rules, max players, and scheduling
- Tournament lifecycle management (UPCOMING → ACTIVE → FINISHED)
- Participation system (submit a post to join a tournament)
- Like-based voting system with consistency constraints
- Automatic winner selection at the end of the tournament
- Winner highlight system (“Last week’s winner”)

#### Content Structuring & Feed Logic

- Ensured tournament posts are excluded from the main feed and friends feed
- Created a dedicated tournament posts section on user profiles
- Added tournament context to posts (display of tournament theme)

#### Authentication & Security

- Implemented registration and login system
- Password hashing and secure storage
- Validation to prevent duplicate users in the database
- Enforced strong password requirements

#### Database & Backend Setup

- Designed the complete Prisma database schema
- Set up Prisma ORM for secure database access
- Created all Dockerfiles and docker-compose from scratch for the full project

#### Frontend & UI/UX

- Defined the overall Direction Artistique (DA) of the application
- Ensured UI consistency across all pages
- Reviewed and refined CSS for all team members’ work
- Designed and implemented:
  - landing page (including animated visuals)
  - default user profile layout
  - language switcher interaction (hover-based UI)

#### Cross-browser Compatibility

- Ensured the application works consistently across multiple browsers (Google Chrome, Mozilla Firefox and Safari)
- Tested and fixed UI/UX inconsistencies
- Multi-browser support required resolving layout issues and differences in rendering

#### Project Setup

- Setup of the frontend and backend architecture (React + NestJS)

## Instructions

# Prerequisites

Make sure the following tools are installed on your machine before getting started:

- docker and docker Compose
- make
- npm

1. Get the repository

You can git clone the repository from the link provided in the evaluation sheet.

2. Set up environment

You can set up the necessary environment variables by copying the contents of the .env.example file located at the root of the repository, and adding the values we will give you into a .env file that you can create in the backend repository

3. Start the project

Simply the command "make start" in a terminal you opened from the root of the repository

4. Acces all the available features

Once the application is running, you can either create your own user account directly, or use one of our pre-created accountes available in the database:

- Regular users: ari, cha, leo, ophe
- Moderator: mod
- Administrator: admin
- Test user for report and ban system: toxic
  All pre-created acounts share the same password: Password0+

5. View emails during development

All emails sent by the application (2FA codes, ban notifications, report confirmations) can be viewed in the Mailpit web interface at:
- **http://localhost:8026**

This interface captures all emails sent during development and is useful for testing the 2FA and moderation notification systems.

## Resources

### Documentation

The following official and classic documentation was consulted throughout the project:

- https://react.dev/learn
- https://docs.nestjs.com/
- https://www.prisma.io/docs
- https://www.postgresql.org/docs/
- https://tailwindcss.com/docs
- https://docs.docker.com/
- https://jwt.io/introduction