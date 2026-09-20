# e2e test scaffold — copy the CONTENTS of this directory into the root of an
# existing GitHub repo (e.g. test-terraform-apply-job) and push to main.
# Pipelines-as-Code picks up .tekton/ and runs CI: build image -> Docker Hub.
# (CD / tag-bump is intentionally NOT wired here: the app repo doubles as the
# GitOps repo during the E2E, so a tag-bump commit would re-trigger PaC. Wire
# gitops-tag-bump against the real GitOps repo — see shared/cicd-tekton-pipelines.)
#
# Prereqs (all in-cluster, from the workspace):
#   - dockerhub-docker-config secret (docker-build-publish-secrets chart)
#   - kaniko-v1 + git-clone-v1 tasks installed
#     (helmfile apply in shared/cicd-tekton-pipelines)
#   - Repository CR for the repo, or the GitHub App installed + repo created
#     after the app install (auto-configure)
#
# PaC namespace: the PipelineRun runs in its Repository CR's namespace
# (default: tekton-pipelines — single run namespace) and resolves shared tasks
# via the cluster-resolver (allowed: tekton-pipelines). Per-run override:
# 'pipelinesascode.tekton.dev/target-namespace: "<ns>"' (Repository CR must
# exist there; add the ns to the TektonConfig allowed-namespaces list).
#
# TODOs before first run:
#   - <DOCKERHUB_USERNAME> in the kaniko IMAGE param
