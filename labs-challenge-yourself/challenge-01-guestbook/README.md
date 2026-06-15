# Challenge 1: Deploy a Guestbook App

## The Goal

Deploy a simple guestbook application that consists of:
- A **Redis leader** (1 replica) for storing data
- **Redis followers** (2 replicas) that replicate from the leader
- A **frontend** (3 replicas) that talks to Redis

All components need to be able to find each other by name.

## Requirements

- The frontend should use the image `gcr.io/google-samples/gb-frontend:v5`
- The Redis leader should use `redis:6` 
- The Redis followers should use `gcr.io/google-samples/gb-redis-follower:v2`
- The frontend needs an environment variable `GET_HOSTS_FROM=dns`
- The frontend should be accessible from your browser

## Hints

<details>
<summary>Hint 1</summary>

You'll need a Deployment + Service for each component (leader, follower, frontend).

</details>

<details>
<summary>Hint 2</summary>

The Redis leader service should be named `redis-leader` and expose port 6379. The follower service should be named `redis-follower`.

</details>

<details>
<summary>Hint 3</summary>

To access the frontend from your browser, think about what Service type makes ports accessible outside the cluster. The frontend listens on port 80.

</details>

<details>
<summary>Hint 4</summary>

You can use `kubectl get all` to see all resources and debug if something isn't connecting.

</details>

## Success Criteria

- [ ] All pods are running (`kubectl get pods` shows everything `Running`)
- [ ] You can access the guestbook in your browser
- [ ] You can submit a message and see it appear
- [ ] Scaling the frontend to 5 replicas works without issues
