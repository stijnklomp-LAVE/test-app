# e2e test scaffold — copy the CONTENTS of this directory into the root of an
# existing GitHub repo (e.g. test-terraform-apply-job) and push to main.
# Pipelines-as-Code picks up .tekton/ and runs CI: build image -> Docker Hub
# -> bump tag in home-argocd -> ArgoCD auto-sync deploys hello-app.
#
# Prereqs (all in-cluster, from the workspace):
#   - dockerhub-docker-config secret (docker-build-publish-secrets chart)
#   - gitops-pat secret (gitops-tag-bump-secrets chart)
#   - gitops-tag-bump-v1 + kaniko-v1 + git-clone-v1 tasks installed
#     (helmfile apply in shared/cicd-tekton-pipelines)
#   - home-argocd repo exists on GitHub with applications/hello-app/baremetal.yaml
#
# PaC namespace caveat: the PipelineRun runs in the repo's PaC namespace and
# resolves shared tasks via the cluster-resolver (allowed: tekton-pipelines,
# home-infra) — push to a home-infra-whitelisted repo (test-terraform-apply-job),
# or extend the TektonConfig allowed-namespaces list.
#
# TODOs before first run:
#   - <DOCKERHUB_USERNAME> in the kaniko IMAGE param
#   - <GIT_OPS_REPO_URL> in the gitops-tag-bump task params