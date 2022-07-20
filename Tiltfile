SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='gcr.io/boyounggcp/supply-chain/tanzu-java-web-app-dev-ns')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev-ns')

k8s_custom_deploy(
    'workload-name',
    apply_cmd="tanzu apps workload apply -f path-to-workload-yaml --live-update" +
        " --local-path " + LOCAL_PATH +
        " --source-image " + SOURCE_IMAGE +
        " --namespace " + NAMESPACE +
        " --yes >/dev/null" +
        " && kubectl get workload workload-name --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f path-to-workload-yaml --namespace " + NAMESPACE + " --yes" ,
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
        sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('workload-name', port_forwards=["8080:8080"],
    extra_pod_selectors=[{'serving.knative.dev/service': 'workload-name'}])


allow_k8s_contexts('by-work-02-admin@by-work-02')