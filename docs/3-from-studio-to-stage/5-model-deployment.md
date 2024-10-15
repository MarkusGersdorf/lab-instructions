## Model Deployment via GitOps

We deployed our `jukebox` model in experiment environment manually, but for the higher environments we need to store the definitions in Git and deploy our models via Argo CD to get all the benefits that GitOps brings.

# Deploying Jukebox 

1. In your code server IDE, open `mlops-gitops/values.yaml` file and **swap** `enabled: false` to `enabled: true` as shown below for each of the app-of-pb definitions:

    <div class="highlight" style="background: #f7f7f7">
    <pre><code class="language-yaml">
        # Test App of Apps for Models
        - name: model-deployments
          enabled: true
          source_path: "."
          helm_values:
            - test/values.yaml
        # Staging App of Apps for Models
        - name: model-deployments
          enabled: true
          source_path: "."
          helm_values:
            - stage/values.yaml
    </code></pre></div>

2. Let's add `jukebox` model definition under `model-deployments/test/values.yaml` and `model-deployments/stage/values.yaml` files as follow. This will take a model deployment helm-chart from a generic helm chart repository and apply the additional configuration to it from the `values` section. 

    ```yaml
      # Jukebox
      jukebox:
        name: jukebox
        enabled: true
        source: https://<GIT_SERVER>/<TEAM_NAME>/mlops-helmcharts.git
        source_ref: main
        source_path: charts/model-deployment
        values:
            model_path: models/jukebox
            model_version: 1
    ```
3. Let's get this deployed of course - it's not real unless its in git!

    ```bash#test
    cd /opt/app-root/src/mlops-gitops
    git add .
    git commit -m  "🐰 ADD - app-of-apps and jukebox to test 🐰"
    git push 
    ```

4. With the values enabled, and the first application listed in the test environment - let's tell ArgoCD to start picking up changes to these environments. To do this, simply update the helm chart we installed at the beginning of the first exercise:

    ```bash#test
    cd /opt/app-root/src/mlops-gitops
    helm upgrade --install argoapps --namespace <TEAM_NAME>-mlops .
    ```

5. You should see the two Jukebox application, one for `test` and one for `stage` deployed in Argo CD. 

6. As you see the `InferenceService` is having a hard time to start, and it is because there is no model saved in the Minio bucket yet. This time we are not going to do it manually, but we are going to let ✨ _pipeline_ ✨ do it for us.