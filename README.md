# 🌩️ CloudLaunch Project

CloudLaunch is a lightweight platform designed to showcase a basic company website and store private internal documents.  
This project demonstrates AWS fundamentals using **S3, IAM, and VPC** with a focus on secure access controls.

---

## 🚀 Task 1: Static Website Hosting Using S3 + IAM
We created and configured three S3 buckets with different purposes and access controls.

### 🔹 Buckets
1. **cloud-launch-site-2025**
   - Hosts a static website (HTML/CSS/JS).
   - Configured for static website hosting.
   - Publicly accessible (read-only).
   - ✅ Live link: [CloudLaunch Website](http://cloud-launch-site-2025.s3-website-eu-west-1.amazonaws.com)

2. **cloud-launch-private-2025**
   - Stores private internal documents.
   - Not publicly accessible.
   - IAM user has `GetObject` and `PutObject` permissions (no delete).

3. **cloud-launch-visible-2025**
   - Not publicly accessible.
   - IAM user can only **list** this bucket (see its existence) but cannot access contents.

### 🔹 IAM User
- **User:** `cloud-launch-user`
- Permissions:
  - `ListBucket` on all three buckets.
  - `GetObject/PutObject` only on `cloud-launch-private-2025`.
  - `GetObject` on `cloud-launch-site-2025`.
  - No delete permissions.
  - No access to contents of `cloud-launch-visible-2025`.
- Policy was written in **JSON** and attached to the user.

---

## 🌐 Task 2: VPC Design
A secure and logically separated VPC was designed for CloudLaunch.

### 🔹 VPC Details
- **VPC Name:** `cloud-launch-vpc`
- **CIDR Block:** `10.0.0.0/16`

### 🔹 Subnets
- Public Subnet: `10.0.1.0/24`
- Application Subnet: `10.0.2.0/24`
- Database Subnet: `10.0.3.0/28`

### 🔹 Internet Gateway
- **Name:** `cloud-launch-igw`
- Attached to `cloud-launch-vpc`.

### 🔹 Route Tables
- **cloud-launch-public-rt**: Routes internet-bound traffic `0.0.0.0/0` → IGW.
- **cloud-launch-app-rt**: Private, no internet route.
- **cloud-launch-db-rt**: Private, no internet route.

### 🔹 Security Groups
1. **cloud-launch-app-sg**
   - Allows HTTP (port 80) within the VPC.
2. **cloud-launch-db-sg**
   - Allows MySQL (port 3306) from the app subnet only.

### 🔹 IAM Permissions
- The IAM user `cloud-launch-user` has **read-only access** to VPC and its components  
  (subnets, route tables, security groups).

---

## 📂 Repository Contents
- `index.html` → Static website homepage.  
- `styles.css` → Styling for the site.  
- `json` → Custom JSON IAM policy for `cloudlaunch-user`.  
- `README.md` → Project documentation.

---

## ✅ Summary
- Static site hosted securely on S3.  
- Private bucket access restricted to IAM user only.  
- Custom IAM JSON policy ensures **least privilege**.  
- VPC designed with **public + private subnets** for future scalability.  

---

### 🌐 Live Website
👉 [http://cloud-launch-site-2025.s3-website-eu-west-1.amazonaws.com](http://cloud-launch-site-2025.s3-website-eu-west-1.amazonaws.com)
