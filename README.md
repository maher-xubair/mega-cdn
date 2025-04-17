# ***MegaCDN***

***A lightweight and serverless CDN utilizing MEGA for file storage and delivery.***

## ***Features***

- *Use MEGA as your CDN backend for file storage and delivery*
- *Multiple account support (load balancing)*
- *Multiple Files and folder Upload Support*
- *Professional and modern UI*
- ***Optional upload-protection** using `Authorization` Header*
- ***Optional auto-deletion** with configurable time periods*
- *Flexible database options: `MongoDB` or `JSON`*
- *Ready for serverless deployment*


## ***Deployments***

- *1. Create **[Account](https://signup.heroku.com)** and deploy on **[Heroku](https://heroku.com/deploy?template=https://github.com/maher-xubair/mega-cdn)***
- *2. Create **[Account](https://vercel.com/signup)** and deploy on **[Vercel](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fmaher-xubair%2Fmega-cdn&env=MEGA_ACCOUNT,PORT,TEMP,MAX_REQUESTS,RATE_LIMIT,AUTO_DELETE,DELETE_TIME,MONGODB_URI,AUTHORIZATION,AUTH_TOKEN,MAX_FILE_SIZE,MAX_FILES,CACHE_TTL&envDescription=Required%20env%20variables%3A%0A-%20MEGA_ACCOUNT%3A%20email%3Apass%3Bemail%3Apass%0A-%20PORT%3A%203000%0A%0AOptional%20env%20variables%3A%0A-%20TEMP%3A%20memory%20or%20file%0A-%20AUTO_DELETE%3A%20true%2Ffalse%0A-%20AUTH_TOKEN%3A%20Your%20Bearer%20Token%20(if%20AUTHORIZATION%3Dtrue)%0A-%20MAX_FILE_SIZE%3A%20In%20MB%0A-%20CACHE_TTL%3A%20In%20seconds)***
- *3. Create **[Account](https://dashboard.render.com/register)** and deploy on **[Render](https://render.com/deploy?repo=https://github.com/maher-xubair/mega-cdn)***
- *4. Create **[Account](https://app.koyeb.com/auth/signup)** and deploy on **[Koyeb](https://app.koyeb.com/deploy?type=git&repository=github.com/maher-xubair/mega-cdn&name=mega-cdn&builder=buildpack&env[MEGA_ACCOUNT]=email:pass;email:pass&env[PORT]=3000&env[TEMP]=memory&env[MAX_REQUESTS]=100&env[RATE_LIMIT]=1%20minute&env[AUTO_DELETE]=false&env[DELETE_TIME]=1440&env[MONGODB_URI]=null&env[AUTHORIZATION]=false&env[AUTH_TOKEN]=YOUR_BEARER_TOKEN&env[MAX_FILE_SIZE]=100&env[MAX_FILES]=10&env[CACHE_TTL]=3600)***
- *5. Create **[Account](https://railway.com)** and deploy on **[Railway](https://railway.com/new)***

## ***Installation & Usage***
<details>
    <summary><strong><em>Installation & Usage</em></strong></summary>

### ***Clone the Repository***

```sh
git clone https://github.com/maher-xubair/mega-cdn.git
cd mega-cdn
npm install
```

### ***Configuration***

*Modify `config.js` or use environment variables. Example `.env` file:*

```
PORT=3000                           # Port to run the app
MEGA_ACCOUNT=email:pass;email:pass  # Multiple accounts for load balancing
TEMP=memory                         # Upload storage option
AUTO_DELETE=true                    # Enable/disable auto-deletion
DELETE_TIME=1440                    # Minutes until deletion (default: 1 day)
MONGODB_URI=null                    # Optional MongoDB connection string
AUTHORIZATION=true                  # Enable/disable secure uploading
AUTH_TOKEN=YOUR_BEARER_TOKEN        # Bearer token for authentication
MAX_FILES=10                        # Maximum files per upload
MAX_FILE_SIZE=50                    # Maximum file size in MB
CACHE_TTL=3600                      # Cache duration in seconds
MAX_REQUESTS=100                    # Max upload requests in specific time
RATE_LIMIT=1 minute                 # 100 req per minute
```
*Note: If you change `AUTH_TOKEN` then update it in public/script.js at line `189`*

### ***Starting the Server***

```sh
npm start          # Start using PM2
npm stop           # Stop the service
npm restart        # Restart the service
```

## ***File Upload API***

### ***Upload Modes***

- ***Single Mode**: Standard upload to any available account*
- ***Dual Mode**: Specify which MEGA account to use*

### ***API Parameters***

| ***Parameter*** | ***Description*** |
|-----------|-------------|
| *`file`*    | *File(s) to upload* |
| *`mode`*    | *`single` (default) or `dual`* |
| *`email`*   | *Target account email (required for `dual` mode)* |

### ***Example Response***

```json
{
  "success": true,
  "files": [
    {
      "url": "https://your_domain.com/media/your_file",
      "name": "your_file.png",
      "size": 51470,
      "formattedSize": "50.26 KB",
      "expires": "86400 sec",
      "formattedExpires": "1 day"
    }
  ]
}
```

## ***Usage Examples***

<details>
  <summary> <strong><em>cURL Examples</em></strong> </summary>

```sh
# Single file without authorization
curl -X POST -F "file=@image.jpg" -F "mode=single" http://yourdomain.com/upload

# Multiple files with authorization
curl -X POST -H "Authorization: Bearer YOUR_TOKEN" \
  -F "file=@image1.jpg" -F "file=@image2.jpg" \
  -F "mode=single" http://yourdomain.com/upload

# Dual mode with email specification
curl -X POST -F "file=@image.jpg" -F "mode=dual" \
  -F "email=user@example.com" http://yourdomain.com/upload
```
</details>

<details>
  <summary> <strong> <em>Node.js Examples</em> </strong> </summary>

```js
// Single file upload
const form = new FormData();
form.append("file", fs.createReadStream("image.jpg"));
form.append("mode", "single");

const response = await axios.post("http://yourdomain.com/upload", form, {
  headers: form.getHeaders()
});

// Multiple files with authentication
const form = new FormData();
form.append("file", fs.createReadStream("image1.jpg"));
form.append("file", fs.createReadStream("image2.png"));
form.append("mode", "dual");
form.append("email", "your@mega-account.com");

const headers = {
  ...form.getHeaders(),
  Authorization: "Bearer YOUR_BEARER_TOKEN"
};

const response = await axios.post("http://yourdomain.com/upload", form, { headers });
```
</details>

</details>


## ***To-Do***

- [ ] *Add custom file name*
- [ ] *Proper logging (error and alerts)*

## ***Contributing***

1. *Fork the repository*
2. *Create a new branch (`feature-web`)*
3. *Commit your changes*
4. *Open a pull request*

## ***Copyright***
*This repository is an enhanced and improved version of the original repository. Although the script and functionality are the same.* <br>
***Credits** to the original **[Repository](https://github.com/IRON-M4N/MegaCDN)** and **[Owner](https://github.com/IRON-M4N)***
