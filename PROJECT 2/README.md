## GIT PROJECT
### Prerequsites
##### . Install git
##### . Create a Github account or sign into a Git account
### Initializing a repository and making commits
Git is a Distributed version control system
```javascript
mkdir Git-Repo
```
```javascript
cd Git-Repo
```
```javascript
git init
```
![Alt text](<images/git init.PNG>)

## Making First Commit
Commit is to save changes made to a file.
```javascript
touch index.html
```
```javascript
git add .
```
```javascript
git commit -m "initial Commit"
```
![Alt text](<images/git commit.PNG>)

## Working with branches
Make your first git branch
```javascript
git checkout -b NewBranch
```
Listing your branches
```javascript
git branch
```
Switch to the old branch
```javascript
git checkout main
```
![Alt text](<images/Working with Branches.PNG>)

Merge a branch into another branch

```javascript
git merge NewBranch
```
To delete a branch
```javascript
git branch -d NewBranch
```
![Alt text](<images/Branch merge & Delete.PNG>)

## Collaboration and remote repositories
Create github account pushing your local git repository to your remote github repository.

![Alt text](<images/Create a Repo in GitHub.PNG>)

```javascript
git config --global user.name "Kingsley Ezebosi"
```
```javascript
git config --global user.email "keze1000@gmail.com"
```
```javascript
git remote add origin https://github.com/kingsleyezebosi/Project_2.git
```
```javascript
git push origin main
```
![Alt text](<images/git push.PNG>)

## Cloning git remote repository
Cloning is to copy the remote repository unto our computer
```javascript
git clone https://github.com/kingsleyezebosi/Project_2.git
```
![Alt text](<images/git clone.PNG>)

# Branch Management and tagging
# Heading 1
## Heading 2
### Heading 3

2. Emphasis
###### *italics* _italics_
###### **bold** __bold__

3. Unordered item list
- Item 1
- Item 2
- Item 3

4. Ordered item list
1. First item
2. Second item
3. Third item

5. Create a HyperLink
###### Vist dare's website - (https://www.dare.io)

6. Display images
###### Run ![Alt Text](https://example.com/image.jpg)
![Alt text](<images/Display Images.PNG>)

7. To Displace code snippet.
###### console.log('Welcome to darey.io')




















































































