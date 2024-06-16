Hi Hari,

For our intranet application, we can manage different environment configurations by creating separate environment files for each environment. When there is a change in the URL for any environment, such as SIT, we update the environment.SIT.ts file and create a new build for that environment.

Environment Configuration
Create environment files:

environment.ts (default environment)
environment.prod.ts (production environment)
environment.SIT.ts (SIT environment)
Modify angular.json to include SIT configuration:
Add a configuration for SIT with the necessary file replacements.

Building for SIT Environment
To create a build for the SIT environment, run the following command:

ng build --configuration=SIT
Deployment
Once the build is complete, deploy the output to your intranet server as per your deployment process.

Usage in Angular Service
Use the environment configuration in your Angular service to access API URLs and other settings.

CI/CD Pipeline with Environment Variables
For dynamically setting these variables during CI/CD, you can use environment variables in your pipeline scripts if you do not want to create a build for any configuration changes.

Define environment variables.
Install dependencies and build the project.
Set environment variables during the build process.
Archive and publish the build artifacts.
Additional Note
It appears that your experience with client-side applications might not be extensive. I would recommend training yourself further on this topic before we discuss it in more detail. This would help avoid any misunderstandings and ensure more productive conversations.

Also, I would encourage you to undergo training on how to build and deploy client-side apps, especially Angular, without creating a new build using environment variables.

Please let me know if you need further details.

Best regards,
Niraj
