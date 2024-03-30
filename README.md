## Introduction
This repo is Reference Seller App with micro-service architecture which contains
- [Seller-app](https://github.com/ONDC-Official/seller-app.git) - Node.js
- [Seller-app-frontend](https://github.com/ONDC-Official/seller-app-frontend.git) - Frontend application (react) being served via [nginx](https://www.nginx.com).
- [Seller-app-protocol](https://github.com/ONDC-Official/seller-app-protocol.git) - Protocol layer(python).
- [Seller-app-igm](https://github.com/ONDC-Official/seller-app-igm.git) - IGM layer(node).
- [Seller-bugzilla-service](https://github.com/ONDC-Official/seller-bugzilla-service.git) - Bugzilla layer(node).

## Usage
1. Make sure you've pulled all sub-directories.

```
   git submodule init
   git submodule update

If the above commands through any error you can try using the below command 

   git submodule update --force --recursive --init --remote
```

- **Populate the .env file with environment variables as described in the below steps**

2. Firebase Authentication.
   - Create the application under [firebase console](http://console.firebase.google.com).
   - Once the application is created, visit the application and click on authentication tab.
   - Enable the following sign methods in the authentication sign-in method tab.
   - In project settings create different projects supported for various platforms like Android, iOS and web, (this will help in downloading the config files, required for authentication)

```
    REACT_APP_FIREBASE_API_KEY=”API_KEY”
    REACT_APP_FIREBASE_AUTH_DOMAIN=”www.example.com”
```

3. SMTP Requirement

Our seller application requires SMTP configuration to send notification emails either for login one time password, or to recieve welcome emails.

      SMTP_HOST=<SMTP_HOST>
      SMTP_PORT=<SMTP_PORT>
      SMTP_AUTH_USERNAME=<SMTP_AUTH_USERNAME>
      SMTP_AUTH_PASSWORD=<SMTP_AUTH_PASSWORD>
      SMTP_EMAIL_SENDER=<SMTP_EMAIL_SENDER>

4. S3 Requirement

Our seller application requires S3 configuration as per our implementation to store the assets which we need for application logics.
   - Login on [AWS](https://aws.amazon.com), and sign in to the console at
   - Navigate to the S3 service and create a bucket with the default settings but enable versioning.
   - Go the bucket's permissions -> uncheck block public access -> uncheck all boxes ->
   - Edit the bucket's policy to be the following:
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Principal": "*",
			"Action": [
				"s3:GetObject",
				"s3:PutObject",
				"s3:GetBucketPolicy"
			],
			"Resource": [
				"arn:aws:s3:::test-ondc/*",
				"arn:aws:s3:::test-ondc"
			]
		}
	]
}
```
   - Edit the CORS policy to the following:
```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "PUT"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    },
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    },
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "PUT"
        ],
        "AllowedOrigins": [
            "http://localhost"
        ],
        "ExposeHeaders": []
    },
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```
   - Navigate to the IAM service -> Create a new user granting it programmatic access
   - Ensure to "Attach Policies Directly" in Permissions Options; ensure AmazonS3FullAccess is checked
   - Ensure the user's in-line policy to the user is something like:
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
				"s3:PutObject"
			],
			"Resource": [
				"*"
			]
		}
	]
}
```
   - On the created user -> Go to “Security Credentials” -> Find generate or CREATE ACCESS KEY button and select LOCAL CODE
   - Check “I understand the above recommendation and want to proceed to create an access key.” and click create. You will get two things ACCESS KEY & SECRET ACCESS KEY.

```
  AWS_ACCESS_KEY_ID=<AWS_S3_ACCESS_KEY>
  AWS_SECRET_ACCESS_KEY=<AWS_S3_ACCESS_KEY>
  S3_VERSION=<S3_VERSION>
  S3_REGION=<S3_REGION>
  S3_BUCKET=<S3_BUCKET>
```

5. Map My India (MMI)

For location based information, integration with MMI has been used. MMI has been used as follows -
Get detailed address information by typing in search query
Get list of addresses for a given PIN code
Get state and city by PIN code
Get Latitude and longitude of the provided address

MMI API that have been used are as follows -
https://outpost.mapmyindia.com/api
https://atlas.mapmyindia.com/api/places/search/json
https://explore.mappls.com
https://apis.mapmyindia.com/advancedmaps/v1
https://atlas.mappls.com/api/places/geocode

```bash
  MMI Configuration in the white labeled buyer app
  MMI_CLIENT_SECRET=”MMI_SERVICE”
  MMI_CLIENT_ID=”MMI_CLIENT_ID”
  MMI_ADVANCE_API_KEY=”MMI_ADVANCE_API_KEY”
```

6. A ngrok instance is required to be running locally to publish to the internet

```
   brew install --cask ngrok
   ngrok http 5555
```

8. Copy the url to the clipboard and paste in BAP_URL and PROTOCOL_BASE_URL in .env-local.

9. Run
   **docker-compose -f docker-compose-for-local.yaml --env-file .env-local up -d**

10. Open [http://localhost](http://localhost).



## User Manual

A detailed user manual for the seller reference app is available [here](https://docs.google.com/document/d/1-8OIo8Ka6Z4ey1amxG_a69lLM0B6tozsWmzwrQmgHKQ/edit).

## ONDC - API Contract for Retail (v1.2.0)
[Documentation](https://docs.google.com/document/d/1brvcltG_DagZ3kGr1ZZQk4hG4tze3zvcxmGV4NMTzr8/edit)

## License

This project is licensed under the ONDC - see the [LICENSE.md](LICENSE.md) file for details.
