## To connect your Namecheap domain to AWS Amplify using Amazon Route 53, follow these steps:

---

### **1. Set Up Your Domain in AWS Amplify**

1. **Deploy Your App in AWS Amplify:**

   - Ensure your app is successfully hosted in AWS Amplify.
   - From the Amplify console, find your app's URL (e.g., `https://appname.amplifyapp.com`).

2. **Add a Custom Domain in Amplify:**
   - In the AWS Amplify console, go to **Domain management**.
   - Click **Add domain**.
   - Enter your domain name (e.g., `example.com`) and proceed.

---

### **2. Set Up Amazon Route 53**

1. **Create a Hosted Zone in Route 53:**

   - Open the **Route 53 console**.
   - Click **Create Hosted Zone**.
   - Enter your domain name (e.g., `example.com`).
   - Set the type to **Public Hosted Zone** and click **Create Hosted Zone**.

2. **Copy the Name Servers (NS):**
   - After creating the hosted zone, Route 53 will generate a set of name server (NS) records.
   - These NS records will look like:
     ```
     ns-1234.awsdns-56.org
     ns-5678.awsdns-78.com
     ns-9012.awsdns-90.net
     ns-3456.awsdns-12.co.uk
     ```

---

### **3. Update Namecheap DNS Settings**

1. **Log in to Namecheap:**

   - Go to the **Domain List** in your Namecheap account.
   - Find your domain and click **Manage**.

2. **Change the Nameservers:**
   - Under the **Nameservers** section, select **Custom DNS**.
   - Enter the Route 53 nameservers (NS records) copied from the previous step.
   - Save the changes.

---

### **4. Configure DNS Records in Route 53**

1. **Add an Alias Record for Amplify:**

   - Go to the **Hosted Zone** for your domain in Route 53.
   - Click **Create Record** and configure the following:
     - **Record Type:** A or CNAME (AWS Amplify typically uses A records).
     - **Name:** Leave it blank to map to the root domain (e.g., `example.com`) or add `www` to map `www.example.com`.
     - **Alias:** Select **Yes**.
     - **Alias Target:** From the dropdown, select your AWS Amplify app.
   - Save the record.

2. **Optionally Add Redirects (e.g., www → root):**
   - Add another record to redirect `www.example.com` to `example.com` (or vice versa) if needed:
     - **Type:** CNAME.
     - **Name:** `www`.
     - **Value:** `example.com`.

---

### **5. Verify Domain Connection**

1. **Wait for DNS Propagation:**

   - It may take up to 24–48 hours for the DNS changes to propagate globally.

2. **Test Your Domain:**
   - Open your browser and navigate to your domain (e.g., `example.com`).
   - Ensure the domain routes correctly to your Amplify app.

---

### **6. (Optional) Configure SSL/TLS for Your Domain**

- AWS Amplify automatically provisions an SSL certificate when you connect a custom domain. Check the domain management section in the Amplify console to confirm the status of the SSL certificate.

---
