## GDSC Certificate Notes

All pods in the gdsc namespace that need to have api access enabled use the gdsc-controller service account with specifications described in kubernetes/accounts/postgis-service-account.yaml. This service account uses the certificates and keys found in kubernetes/pki/ which have generation instruction in the same directory.

Anytime there is a certificate renewal these steps must be followed:
-   regenerate the certificates and keys in kubernetes/pki/ using the instructions in the same directory
-   rebuild the docker image for docker/degaussAPI using the build.sh script in the docker/ directory
-   delete and recreate the degauss-fastapi deployment