# DAY26: Drill — "User Profile Card"

Build a React + TypeScript component in `DAY26/`. Use a Vite scaffold for easy setup.

## Setup

```bash
cd ~/Dev/personal-learning-AI/learn-ts/DAY26
npm create vite@latest . -- --template react-ts
npm install
```

## Requirements

Create these components in `src/`:

### Types (src/types.ts)
```typescript
export interface User {
  id: number;
  name: string;
  email: string;
  avatar?: string;
  bio?: string;
  joined: string;
  isOnline: boolean;
}
```

### UserCard (src/components/UserCard.tsx)
- Props: `user: User`
- Shows avatar (or initials fallback), name, email, bio, join date
- Shows a green dot if online, gray if offline
- Style with inline styles or a CSS module

### UserList (src/components/UserList.tsx)
- Props: `users: User[]`, `onSelect: (user: User) => void`
- Renders a list of UserCards
- Clicking a card calls `onSelect`

### App.tsx
```typescript
function App(): JSX.Element {
  const [selectedUser, setSelectedUser] = useState<User | null>(null);
  // Hard-code 3-4 sample users
  // Display UserList and the selected user details
}
```

## Expected output

A page showing user cards. Click a card → it highlights and shows details. If none selected, show "Select a user".

```tsx
// Sample users:
const sampleUsers: User[] = [
  {
    id: 1,
    name: "Alice Johnson",
    email: "alice@example.com",
    bio: "Full-stack developer",
    joined: "2024-01-15",
    isOnline: true,
  },
  // ... add more
];
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `img` tag for avatar, or show initials: `user.name.charAt(0).toUpperCase()`
- Green dot: a `span` with `backgroundColor: "green"` and `borderRadius: "50%"`
- Selected state: compare `selectedUser?.id === user.id` for highlighting
- CSS: keep it simple with inline styles or a `styles.module.css`
- `npm run dev` starts the Vite dev server
</details>

## Bonus

Add a search input that filters users by name. Add a "UserForm" component that can create new users (add to the list with a form).

## When you're done

Show me your component files and a screenshot of the running app.
