# Label and Selectors

```bash
k get pods --selector env=dev

k get pods --selector bu=finance

k get all --selector env=prod

k get pods --selector env=prod,bu=finance,tier=frontend
```