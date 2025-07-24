# 🧹 Auto Delete EC2 Snapshots with Lambda on Instance Termination

This setup creates a Lambda function that automatically deletes EBS snapshots if the EC2 instance associated with the volume is terminated.

---

## ✅ Prerequisites

- An EC2 instance with its default attached EBS volume
- At least one snapshot created from the volume
- <img width="940" height="130" alt="image" src="https://github.com/user-attachments/assets/5a91dd3d-65e9-4a68-82be-7bae4d7600ab" />

- Lambda function code present as `ebs_stale_snapshosts.py` in your repository

---

## ⚙️ Step 1: Create Lambda Function

1. Go to **Lambda > Create Function**
2. Choose **Author from scratch**
3. Function name: `cost-optimization-lambda`
4. Runtime: Python 3.x
5. Permissions: Select **Create a new role with basic Lambda permissions**
6. After creation, open the function and replace the default code with the content from your `ebs_stale_snapshosts.py` file
7. Click **Deploy and then Test** 

<img width="1514" height="598" alt="image" src="https://github.com/user-attachments/assets/d53303cd-e192-43ff-b692-434258dec207" />

---

## 🔐 Step 2: Update Lambda Execution Role with Required Permissions

1. Go to **IAM > Roles**
2. Search for the role automatically created for your Lambda (name starts with `lambda-role-...`)
3. Click **Add permissions > Create inline policy**
4. Choose **Service**: EC2
5. Actions to allow:
   - `DescribeInstances`
   - `DescribeVolumes`
   - `DescribeSnapshots`
   - `DeleteSnapshot`
6. Resources: All
7. Review and create policy

   <img width="1501" height="579" alt="image" src="https://github.com/user-attachments/assets/c5fe95bc-9a58-46bb-ab6a-b75c9db03cd9" />

   <img width="940" height="235" alt="image" src="https://github.com/user-attachments/assets/04400d80-df6e-4ea7-8f15-3f802aeeec92" />

   


---

## ⏱️ Step 2(a): Increase Lambda Timeout to 10 Seconds

1. In the Lambda console, go to the **Configuration** tab
2. Click **General configuration > Edit**
3. Change **Timeout** to `10 seconds`
4. Save changes

---

## 🧪 Step 3: Test the Lambda Function

- Run the Lambda function while the EC2 instance is still running → The snapshot **will not** be deleted

- <img width="940" height="128" alt="image" src="https://github.com/user-attachments/assets/07db11f7-5687-4341-a829-d2687635d460" />

- Terminate the EC2 instance and run the Lambda again → The snapshot **will be deleted automatically**

- <img width="940" height="191" alt="image" src="https://github.com/user-attachments/assets/3698b45b-ee37-445f-bd34-fb78175ae948" />

<img width="940" height="126" alt="image" src="https://github.com/user-attachments/assets/a732886a-3ffc-4053-bd9a-3f89cc11594b" />


