# Challenge 3: Blue-Green Deployment

## The Goal

Perform a blue-green deployment: run two versions of an app simultaneously, then switch traffic from the old version ("blue") to the new version ("green") instantly - with zero downtime.

## Scenario

- You have a "blue" app serving `Version 1.0`
- You need to deploy a "green" app serving `Version 2.0`
- A single Service should control which version receives traffic
- You must be able to switch back to blue instantly if something goes wrong

## Requirements

- Use the image `hashicorp/http-echo` with different `-text` args for each version
- Both versions should be running at the same time
- Only ONE version should receive traffic at any given time
- Switching between versions should be instant (no pod restarts)

## Hints

<details>
<summary>Hint 1</summary>

You'll need two separate Deployments - one for blue, one for green. They should have different label values to distinguish them.

</details>

<details>
<summary>Hint 2</summary>

The Service's `selector` determines which pods get traffic. Think about what happens if you change the selector to point to a different label value.

</details>

<details>
<summary>Hint 3</summary>

You can edit a Service's selector with `kubectl patch` or by applying an updated YAML. This is how you "flip" traffic.

</details>

<details>
<summary>Hint 4</summary>

Test with a loop: `while true; do curl <service-url>; sleep 1; done` - you should see the response change instantly when you switch.

</details>

## Success Criteria

- [ ] Both blue and green deployments are running simultaneously
- [ ] Traffic initially goes to blue (Version 1.0)
- [ ] After switching, traffic goes to green (Version 2.0)
- [ ] You can switch back to blue without restarting anything
- [ ] No requests are dropped during the switch
