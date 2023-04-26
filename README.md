# Homework

## Instructions

To run the deployment, follow these steps:

1. Fork the repo.
2. Create the following secrets in your repo:
    * `USERNAME`=[your github username]
    * `PAT`=[GitHub PAT with read and write package permission]
    * `MYSQL_PASSWORD`=[MySQL root password]
3. The CD workflow is triggered by a commit to main, or you can start it manually through the workflow_dispatch trigger.

## Run Locally

1. Clone the repo locally.
2. Setup a Kind cluster locally: `kind create cluster`
3. Install the Helm chart

    ```bash
    helm dependency build ./chart
    helm upgrade --install homework ./chart \
        --namespace homework \
        --create-namespace \
        --values chart/values.yaml \
        --set mysql.auth.rootPassword=test_password \
        --set tag=${{ github.run_id }} \
        --set image=timbuchinger/homework \
        --wait
    ```

4. Port forward the service for MySQL: `kubectl port-forward svc/homework-mysql 3306:3306`
5. Install the database schema: `mysql -u root -p"${{ secrets.MYSQL_PASSWORD }}" -h 127.0.0.1 homework < sql/users.sql`
6. Port forward the service for the API and execute commands!

    ```bash
    kubectl port-forward svc/python-service 8000:8000
    curl -X GET http://localhost:8000
    curl -X POST -H "Content-Type: application/json" -d '{"name":"tim","email":"noreply@email.com","pwd":"my_password"}' http://localhost:8000/create
    curl -X GET http://localhost:8000/users
    ```
