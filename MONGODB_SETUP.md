# MongoDB Atlas Setup Guide

This guide will walk you through creating a MongoDB database in MongoDB Atlas (free cloud option).

## Step 1: Create a MongoDB Atlas Account

1. Go to [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Click "Sign Up" (or "Sign in" if you have an account)
3. Create account with:
   - Email: Use your email
   - Password: Create a strong password
   - Company: "Personal" or your name
4. Click "Create Free Account"
5. Verify your email address

## Step 2: Create a Project

1. After signing in, you'll see "Create a Project"
2. Project Name: `budget-planner` (or any name)
3. Click "Create Project"

## Step 3: Create a Cluster

1. Click "Create a Cluster"
2. Select "M0 Free" tier (it's free forever for learning)
3. Choose your preferred region (any region is fine)
4. Cluster Name: `budget-cluster` (or any name)
5. Click "Create Deployment"
6. Wait for cluster to create (usually 1-2 minutes)

## Step 4: Create Database User

1. In the left menu, click "Database Access"
2. Click "Add New Database User"
3. **Username:** `budgetuser` (remember this!)
4. **Password:** Create a strong password (click "Generate" for auto-generate) - **Remember this!**
5. **Built-in Roles:** Select "readWriteAnyDatabase"
6. Click "Add User" at bottom

## Step 5: Allow Network Access

1. In the left menu, click "Network Access"
2. Click "Add IP Address"
3. Click "Allow Access from Anywhere" (for development)
   - This adds `0.0.0.0/0` which allows connections from any IP
   - ⚠️ For production, use your specific IP address
4. Click "Confirm"

## Step 6: Get Your Connection String

1. Go back to "Database" in left menu
2. Find your cluster and click "Connect"
3. Click "Drivers"
4. Select "Node.js" and version "4.x or higher"
5. Copy the connection string - it looks like:
   ```
   mongodb+srv://budgetuser:<password>@budget-cluster.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```

6. **Replace `<password>` with your actual password** from Step 4

## Step 7: Update Your .env File

In your budget folder, create/update `.env` file with:

```
# MongoDB Connection String (from Step 6)
MONGODB_URI=mongodb+srv://budgetuser:YOUR_PASSWORD_HERE@budget-cluster.xxxxx.mongodb.net/budget-planner?retryWrites=true&w=majority

# GitHub Configuration
GITHUB_TOKEN=ghp_your_token_here
GITHUB_REPO_OWNER=your_github_username
GITHUB_REPO_NAME=your_repository_name

# Server Configuration
PORT=5000
NODE_ENV=development
```

**Important:**
- Do NOT share your `.env` file
- The `.env` file is in `.gitignore` so it won't be committed to GitHub
- Keep your connection string and passwords private

## Step 8: Test the Connection

1. Open PowerShell in your budget folder
2. Make sure `.env` file is created
3. Run:
   ```bash
   npm install
   npm start
   ```

You should see:
```
MongoDB connected
Server running on port 5000
```

If you see "MongoDB connection error", check:
- Username and password are correct
- Password is properly escaped (if it has special characters like @, use URL encoding)
- IP Access is set to allow (0.0.0.0/0)
- Connection string has `/budget-planner` at the end (database name)

## Step 9: View Your Data

After submitting feedback:

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Click your cluster
3. Click "Collections"
4. You should see a `budget-planner` database with a `feedbacks` collection
5. Click the collection to see your feedback entries

## That's It!

Your feedback system is now:
✅ Saving to MongoDB database
✅ Creating GitHub Issues
✅ Displaying feedback on your website

## Troubleshooting

**Problem: "MongoDB connection error"**
- Solution: Check your MONGODB_URI in .env file
- Make sure username/password are correct
- Verify IP Access is set to 0.0.0.0/0

**Problem: "Connection refused"**
- Solution: Wait a few minutes for cluster to fully initialize
- Check that network access is allowed

**Problem: "ECONNREFUSED"**
- Solution: Make sure you're using MongoDB Atlas connection string (mongodb+srv://...) not localhost

**Problem: Can't find password****
- Solution: Go to Database Access on MongoDB Atlas and reset the password for your user
