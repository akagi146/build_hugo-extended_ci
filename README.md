# build_hugo-extended_ci

## Overview

Deploy on Cloud-Build with Hugo-Extended and build a CI that deploys the product to Firebase-Hosting.

## Description

[GoogleCloudPlatform/cloud-builders-community][1] Create Hugo-Extended buildable image based on Docker image published in repository.

There is no problem with the original Docker image under normal circumstances, but if you want to use a theme that uses SASS / SCSS, an error will occur during build execution.

Create a Docker image that uses Hugo-Extended so that you can build without problems even with themes that use SASS / SCSS.

Therefore, the content of "hugo / Dockerfile" is customized for Hugo-Extended.

At the same time, it stores "cloudbuild.yaml" that describes the CI processing sample to be run by Cloud-Build.

## Premise

1. [Google Cloud SDK][2] is installed on the device to work.
1. GCP is available
	* Firebase Hosting Available
1. I have created a private repository on GitHub to manage the Hugo content I want to build.

## Usage

1. Container creation used by Cloud Build

	```
	$ cd ./firebase
	$ gcloud builds submit --config cloudbuild.yaml .
	$ cd ../hugo
	$ gcloud builds submit --config cloudbuild.yaml .
	```

1. Create a token for Firebase Hosting

	```
	$ firebase login:ci
	Visit this URL on any device to log in:
	https://accounts.google.com/o/oauth2/auth?client_id= ...(and more）...

	Waiting for authentication...

	＋  Success! Use this token to login on a CI server:

	1/*******************　⇐ (token)

	Example: firebase deploy --token "$FIREBASE_TOKEN"
	```

1. See [this page][3] to encrypt tokens for Firebase Hosting.

1. Create a trigger that fires when the repository that manages Hugo content is updated in the Cloud Build administration screen.
	* Specify "cloudbuild.yaml" in the Cloud Build configuration file.

1. Put "cloudbuild.yaml" in Hugo content and reflect it remotely.
	* For the contents of "cloudbuild.yaml", refer to the ones stored in this repository.
	* Remarks: 
		* "Cloudbuild.yaml" registered in the repository is a pattern in which theme files are managed in another private repository separately from the repository that manages Hugo content.
		* Therefore, the secret key for accessing the repository that manages the theme is encrypted and placed in the repository that manages Hugo content.
		* If you use the theme of the public repository, or manage the theme in the same repository, please modify as appropriate according to the use case.

## Licence
This source code is licensed under Apache 2.0. Full license text is available in [Here][4].

# Authors
* Creation of Original: [GoogleCloudPlatform][5]
	* Modified file: hugo/Dockerfile
* File created by me: ./cloudbuild.yaml

[1]: https://github.com/GoogleCloudPlatform/cloud-builders-community
[2]: https://cloud.google.com/sdk/docs/
[3]: https://cloud.google.com/cloud-build/docs/securing-builds/use-encrypted-secrets-credentials
[4]: http://www.apache.org/licenses/LICENSE-2.0
[5]: https://github.com/GoogleCloudPlatform
[6]: https://github.com/akagi146
