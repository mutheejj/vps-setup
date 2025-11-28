# How to Create a Domain and Subdomain on Name.com

This guide provides simple steps for registering a domain and creating subdomains using Name.com.

---

## 1. How to Register a Domain on Name.com

### **Step 1: Visit Name.com**

1. Go to **[https://www.name.com](https://www.name.com)**.
2. Use the search bar to check domain availability.

### **Step 2: Purchase the Domain**

1. If your domain is available, click **Add to Cart**.
2. Proceed to **Checkout**.
3. Create an account or log in.
4. Complete payment.

Your domain is now registered.

---

## 2. How to Create a Subdomain on Name.com

### **Step 1: Open DNS Management**

1. Log into your Name.com account.
2. Go to **My Domains**.
3. Select the domain you want to manage.
4. Click **DNS Records**.

### **Step 2: Add a Subdomain Record**

Click **Add DNS Record**, then fill in:

#### **A Record Example (subdomain → IP address)**

```
Type: A
Host: subdomain   (example: api, blog, app)
Answer: your-server-ip
TTL: Default or 300
```

Click **Add Record**.

#### **CNAME Example (subdomain → another domain)**

```
Type: CNAME
Host: subdomain
Answer: example.com
TTL: Default or 300
```

Click **Add Record**.

---

## 3. Verify the Subdomain

After DNS propagation (5–30 minutes), visit:

```
http://subdomain.example.com
```

Your subdomain is now active.

---

## Notes

* Use **A records** when pointing to a VPS IP.
* Use **CNAME** when pointing to another domain or service.
* DNS changes may take longer globally (up to 24 hours).

