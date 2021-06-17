# full-react-sample

This is a full react mobile App sample using Django in backend and PostgreSQL database.

## Use backend
In `backend` directory create the `.env` file using config below:
```
ENV=dev
DEFAULT_HOSTNAME=
DEFAULT_DATABASE=
DEFAULT_USER=
DEFAULT_PASSWORD=
SECRET_KEY=
```
After run the SQL content that is in `backend/sql.txt` in your PostgreSQL instance.
To run just use the docker with `build.sh` and `run.sh` commands, inside the `backend` directory

## Deploy
Steps taken to deploy this application in Google Cloud (GCP).

### Step1: Cloud NAT
For application security, I configure this NAT.
Thus, all external connections of the application will go through a NAT gateway leaving through an IP managed by Google.
This way the application is masked and cannot be discovered.

### Step2: Cluster Kubernetes
Also with security in mind, a private Kubernetes Cluster was created.

### Step3: Cloud SQL
Instantiate Cloud SQL with PostgreSQL only with private access without public IP

### Step4: Bastion Host
Accessed the PostgreSQL instance of the Cloud SQL through a bastion host and executed the SQL to create the tables.

### Step5: Container Registry
Uploaded docker image to Container Registry
```
cd backend
export GCP_PROJECT_ID=xxxx
docker build -t full-react-sample .
docker tag full-react-sample gcr.io/$GCP_PROJECT_ID/full-react-sample
docker push gcr.io/$GCP_PROJECT_ID/full-react-sample
```

### Step6: Configure Workload, Service and Load Balance
On Google Cloud I configure all components to make the docker application run in the safest way using Google Cloud edge services.
This way it is accessible to the mobile app

### Step7: DNS
Created DNS record to point to Google Load Balance IP