# Setting Up a Separate Backend Repository

This guide shows you how to create a separate repository just for the backend code, which makes deployment easier.

## Why Create a Separate Repo?

- ✅ Easier deployment (Render can access it easily)
- ✅ Can make it public (no class project code exposed)
- ✅ Cleaner separation of concerns
- ✅ Easier to share backend URL with frontend developers

## Step 1: Create New Repository

1. Go to GitHub/GitLab/Bitbucket
2. Click **"New Repository"**
3. Name it: `asa-policy-backend` (or your preferred name)
4. Choose **Public** or **Private** (public is fine since no secrets are in code)
5. **Don't** initialize with README, .gitignore, or license (we'll copy files)
6. Click **"Create repository"**

## Step 2: Copy Backend Files

### Option A: Using Terminal (Recommended)

```bash
# Navigate to your project root
cd /Users/chisomchiobi/Code/Project--7-ASA-Policy-App-2026

# Create a temporary directory for the new repo
mkdir -p ../asa-policy-backend-temp
cd ../asa-policy-backend-temp

# Copy all backend files (excluding venv and __pycache__)
cp -r ../Project--7-ASA-Policy-App-2026/backend/* .
cp -r ../Project--7-ASA-Policy-App-2026/backend/.gitignore . 2>/dev/null || true

# Remove unnecessary files
rm -rf venv __pycache__ app/__pycache__ app/*/__pycache__ 2>/dev/null || true

# Initialize git
git init
git add .
git commit -m "Initial backend setup"

# Add remote (replace with your actual repo URL)
git remote add origin https://github.com/yourusername/asa-policy-backend.git
git branch -M main
git push -u origin main
```

### Option B: Manual Copy

1. Create a new folder on your computer
2. Copy all files from `backend/` folder **except**:
   - `venv/` (virtual environment - don't copy)
   - `__pycache__/` (Python cache - don't copy)
   - `.env` (if it exists - don't copy, use environment variables)
3. Copy these files/folders:
   - `app/` (entire folder)
   - `database/` (entire folder)
   - `main.py`
   - `requirements.txt`
   - `Procfile`
   - `BACKEND.md`
   - `DEPLOYMENT.md`
   - `.gitignore` (if exists)

4. In the new folder, initialize git:
   ```bash
   git init
   git add .
   git commit -m "Initial backend setup"
   git branch -M main
   git remote add origin https://github.com/yourusername/asa-policy-backend.git
   git push -u origin main
   ```

## Step 3: Verify Repository Structure

Your new repository should have this structure:

```
asa-policy-backend/
├── app/
│   ├── core/
│   ├── models/
│   └── routers/
├── database/
│   └── database_schema.sql
├── main.py
├── requirements.txt
├── Procfile
├── BACKEND.md
├── DEPLOYMENT.md
└── .gitignore
```

## Step 4: Deploy to Render

Now you can deploy from this new repository:

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click **"New +"** → **"Web Service"**
3. Select your new `asa-policy-backend` repository
4. Configure:
   - **Root Directory**: Leave empty (root is the backend)
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Add environment variables (see DEPLOYMENT.md)
6. Deploy!

## Keeping Backend Updated

When you make changes to the backend in your main project:

```bash
# In your main project
cd backend
# Make your changes...

# Copy updated files to backend repo
cd ../asa-policy-backend
cp -r ../Project--7-ASA-Policy-App-2026/backend/* .
# Remove venv and cache again
rm -rf venv __pycache__ app/__pycache__ app/*/__pycache__ 2>/dev/null || true

# Commit and push
git add .
git commit -m "Update backend code"
git push
```

Render will automatically detect the push and redeploy!

## Notes

- **No secrets in code**: All sensitive data (Supabase keys) goes in Render's environment variables
- **Public repo is safe**: Since no secrets are in the code, making it public is fine
- **Keep main project private**: Your class project repo can stay private
- **Easy sharing**: You can share the backend repo URL with teammates or instructors
