# To create a pipeline:
1. Create the tasks
    ```
    oc apply -f scan-image/tasks/rhacs-deployment-check.yaml
    oc apply -f scan-image/tasks/rhacs-image-check.yaml
    oc apply -f scan-image/tasks/rhacs-image-scan
    ```
2. Edit pipeline.yaml by replacing the image to scan (image-registry.example-image:latest) with the actual image that is to be scanned.
3. Create the pipeline
    ```
    oc apply -f scan-image/pipeline.yaml
    ```
4. Create the pipelinerun
    ```
    oc create -f scan-image/pipelinerun.yaml
    ```