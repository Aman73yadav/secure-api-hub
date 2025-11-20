# Backend Developer Internship - Project Assignment

## 🎯 Project Overview

This is a **Scalable REST API with Authentication & Role-Based Access** featuring a modern React frontend. The project demonstrates professional backend development practices with secure authentication, database design, and API architecture.

## ✨ Features Implemented

### Backend Features
- ✅ **User Authentication**
  - Email/password registration and login
  - JWT token-based authentication
  - Password hashing for security
  - Auto-confirm email (configurable)

- ✅ **Role-Based Access Control (RBAC)**
  - User role: Can only view/manage their own tasks
  - Admin role: Can view/manage all tasks in the system
  - Security definer functions to prevent RLS recursion
  - Separate roles table (security best practice)

- ✅ **Task Management CRUD API**
  - Create, Read, Update, Delete operations
  - Task fields: title, description, status, priority
  - Status options: pending, in_progress, completed
  - Priority levels: low, medium, high
  - Automatic timestamps (created_at, updated_at)

- ✅ **Security & Validation**
  - Row Level Security (RLS) policies on all tables
  - Input validation using Zod schema validation
  - SQL injection prevention
  - Secure foreign key relationships
  - Proper error handling

- ✅ **Database Design**
  - PostgreSQL database with proper schema
  - Normalized tables with relationships
  - Enums for status and priority
  - Triggers for automatic timestamp updates
  - User role assignment on signup

### Frontend Features
- ✅ **Authentication UI**
  - Login and signup forms
  - Input validation with error messages
  - Secure password handling
  - Session persistence

- ✅ **Dashboard**
  - Task list view with cards
  - Create new tasks
  - Edit existing tasks
  - Delete tasks
  - Color-coded status badges
  - Priority indicators

- ✅ **Admin Panel**
  - View all users' tasks
  - Admin-only access
  - Manage any task in the system

- ✅ **Responsive Design**
  - Modern, professional UI
  - Tailwind CSS styling
  - Shadcn UI components
  - Mobile-friendly layout

## 🏗️ Architecture

### Database Schema

```sql
-- User Roles Table
CREATE TABLE user_roles (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES auth.users(id),
  role app_role (enum: 'user', 'admin'),
  created_at TIMESTAMPTZ
);

-- Tasks Table
CREATE TABLE tasks (
  id UUID PRIMARY KEY,
  title TEXT NOT NULL,
  description TEXT,
  status TEXT (enum: pending, in_progress, completed),
  priority TEXT (enum: low, medium, high),
  user_id UUID REFERENCES auth.users(id),
  created_at TIMESTAMPTZ,
  updated_at TIMESTAMPTZ
);
```

### Row Level Security Policies

**Tasks Table:**
- Users can view only their own tasks
- Admins can view all tasks
- Users can create tasks (auto-assigned to themselves)
- Users can update/delete only their own tasks
- Admins can update/delete all tasks

**User Roles Table:**
- Users can view their own roles
- Admins can view all roles

### API Design

The backend uses Lovable Cloud (powered by Supabase), which provides:
- RESTful API endpoints via Supabase client
- Automatic JWT token management
- Built-in API versioning
- Error handling and validation
- PostgreSQL database with RLS

## 🚀 Getting Started

### Prerequisites
- Node.js (v18 or higher)
- npm or bun

### Installation

1. Clone the repository
```bash
git clone <your-repo-url>
cd <project-name>
```

2. Install dependencies
```bash
npm install
```

3. Start the development server
```bash
npm run dev
```

4. Open http://localhost:8080 in your browser

### Testing the Application

1. **Sign Up**: Create a new account (default role: user)
2. **Login**: Access your dashboard
3. **Create Tasks**: Add tasks with different priorities and statuses
4. **Manage Tasks**: Edit or delete your tasks
5. **Admin Access**: To test admin features, you need to manually assign admin role in the database

### Assigning Admin Role

To test admin functionality, you need to add admin role to a user:

1. Go to Lovable Cloud tab → Database → user_roles table
2. Add a new row with:
   - user_id: (copy from auth.users table)
   - role: admin

## 🔐 Security Features

- **Password Hashing**: Automatic via Supabase Auth
- **JWT Tokens**: Secure, httpOnly cookies
- **Input Validation**: Zod schema validation on all forms
- **SQL Injection Prevention**: Parameterized queries via Supabase client
- **Row Level Security**: Database-level access control
- **CORS**: Configured for secure API access
- **XSS Prevention**: React's built-in protection
- **Session Management**: Secure token refresh

## 📊 Scalability Considerations

### Current Implementation
- Serverless edge functions (auto-scaling)
- PostgreSQL with connection pooling
- JWT stateless authentication
- Row Level Security for multi-tenancy

### Future Enhancements
- **Caching**: Redis for frequently accessed data
- **Load Balancing**: Multiple server instances
- **Database Replication**: Read replicas for scaling
- **CDN**: Static asset distribution
- **Rate Limiting**: API request throttling
- **Monitoring**: Performance metrics and logging
- **Microservices**: Service separation for larger scale
- **Message Queue**: Async task processing

## 📁 Project Structure

```
src/
├── components/          # React components
│   ├── ui/             # Shadcn UI components
│   ├── TaskList.tsx    # User's task list
│   ├── AdminTaskList.tsx # Admin view of all tasks
│   ├── CreateTaskDialog.tsx
│   └── EditTaskDialog.tsx
├── pages/              # Route pages
│   ├── Index.tsx       # Landing page
│   ├── Auth.tsx        # Login/Signup
│   ├── Dashboard.tsx   # User dashboard
│   └── Admin.tsx       # Admin panel
├── integrations/       # Lovable Cloud integration
│   └── supabase/       # Auto-generated Supabase client
└── lib/                # Utility functions
```

## 🛠️ Technologies Used

### Backend
- **Lovable Cloud** (Supabase): Backend infrastructure
- **PostgreSQL**: Database
- **JWT**: Authentication tokens
- **Zod**: Input validation
- **TypeScript**: Type safety

### Frontend
- **React 18**: UI framework
- **TypeScript**: Type-safe code
- **Tailwind CSS**: Styling
- **Shadcn UI**: Component library
- **React Router**: Navigation
- **Vite**: Build tool

## 📝 API Documentation

The API follows REST principles with the Supabase client handling all HTTP communication.

### Authentication Endpoints
- `supabase.auth.signUp()` - Register new user
- `supabase.auth.signInWithPassword()` - Login
- `supabase.auth.signOut()` - Logout
- `supabase.auth.getSession()` - Get current session

### Task Endpoints
- `supabase.from('tasks').select()` - GET all tasks
- `supabase.from('tasks').insert()` - POST new task
- `supabase.from('tasks').update()` - PUT/PATCH update task
- `supabase.from('tasks').delete()` - DELETE task

### Role Management
- `supabase.from('user_roles').select()` - GET user roles
- Uses `has_role()` function for role checks

## 🎓 Assignment Requirements Met

✅ User registration & login APIs with password hashing and JWT authentication  
✅ Role-based access (user vs admin)  
✅ CRUD APIs for tasks (secondary entity)  
✅ API versioning, error handling, validation  
✅ Database schema (PostgreSQL)  
✅ React.js frontend  
✅ Registration & login UI  
✅ Protected dashboard (JWT required)  
✅ CRUD interface for tasks  
✅ Error/success messages from API  
✅ Secure JWT token handling  
✅ Input sanitization & validation  
✅ Scalable project structure  

## 📞 Support

For questions or issues, please refer to the documentation or contact the development team.

---

**Built with ❤️ for Backend Developer Internship Assignment**