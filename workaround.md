# Token Expiration Workaround

## Problem

The Twitch token expires approximately every two months. We need to generate a new token and redeploy the app when the token expires.

## Workaround

### Step 1: Generate a New Token

1. Log in to [Twitch developers console](https://dev.twitch.tv/console) to find your client ID (a unique, constant ID for each developer account) and generate a new secret.

2. Open the Postman Collection [03. Twitch API 1](https://api.postman.com/collections/15044152-130d81e7-e4f2-415a-8988-08ec03ee1dbc?access_key=PMAT-01GTQTXKMBQGCKG9SV78V5M50S) and use the "Twitch Token Generate" request to generate a new token by providing your client ID and secret.

3. Navigate to the `external` folder in the backend directory and open `TwitchClient.java`. Update the token with the new one.

4. Build the project into a WAR file: In IntelliJ for example, select "Build" > "Build Artifacts" > "screenager:war" > "Build".

5. Once the build is complete, go to the target directory and rename the `.war` file to `screenager.war`.

6. Start the Tomcat server locally to test if the project is working again.

### Step 2: Delete Old Images and Redeploy

1. SSH into the virtual machine: `ssh -i /path/to/your/awskey.pem ubuntu@remote_public_dns`.

2. Open a new terminal locally and copy the `.war` file to the virtual machine: `scp -i /path/to/your/awskey.pem /path/to/your/war/file/screenager.war uubuntu@remote_public_dns:~/`.

3. Once the upload is complete, run `sudo docker ps` to check if there are any old instances running. If so, stop them using `sudo docker stop <container_id>` and `sudo docker rm <container_id>`.

4. Check for any old images using `sudo docker images`. If there are any, delete them using `sudo docker rmi <image_name>`.

5. Build a new image: `sudo docker build -t screenager .`.

6. Run the new image: `sudo docker run -d -p 80:8080 screenager`.

7. Visit the deployed website URL to check if the website is working again.