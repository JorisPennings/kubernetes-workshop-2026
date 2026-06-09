# Challenge 2: Fix the Broken Deployment

## The Goal

A colleague left behind a set of manifests that are supposed to deploy a web application with a database. Nothing works. Your job is to figure out what's wrong and fix it.

## Instructions

1. Apply all manifests in this folder:
   ```bash
   kubectl apply -f broken/
   ```
2. Observe what's happening (or not happening)
3. Diagnose and fix each issue
4. Get the app fully functional

## Hints

<details>
<summary>Hint 1</summary>

Start by checking pod status: are they running, pending, or in an error state? Use `describe` to get more details on troubled pods.

</details>

<details>
<summary>Hint 2</summary>

Look carefully at labels and selectors - do the Services match the Pods they're supposed to target?

</details>

<details>
<summary>Hint 3</summary>

Check port numbers. Are the container ports, service ports, and target ports consistent?

</details>

<details>
<summary>Hint 4</summary>

One of the manifests references a ConfigMap that doesn't exist. Another has an image that can't be pulled.

</details>

## Success Criteria

- [ ] All pods are in `Running` state
- [ ] The web app can connect to the database
- [ ] `kubectl logs` shows no errors
- [ ] The service endpoint resolves correctly
