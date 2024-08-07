# K8s_Node_Affinity

Node Selectors are great for basic pod placement based on node labels. But what if you need more control over where your pods land. This feature offers advanced capabilities to fine-tune pod scheduling in your Kubernetes cluster.

Node Affinity lets you define complex rules for where your pods can be scheduled based on node labels. 

#### Key Features:

- Flexibility: Define precise conditions for pod placement.
- Control: Decide where your pods can and cannot go with greater granularity.
- Adaptability: Allow pods to stay on their nodes even if the labels change after scheduling.

#### Properties:

- requiredDuringSchedulingIgnoredDuringExecution
- preferredDuringSchedulingIgnoredDuringExecution

Required During Scheduling, Ignored During Execution: This is the strictest type of Node Affinity

- Specify Node Labels: Define a list of required node labels (e.g., disktype=ssd) in your pod spec.
- Exact Match Requirement: The scheduler only places the pod on nodes with those exact labels.
- Execution Consistency: Once scheduled, the pod remains on the node even if the label changes.

---

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

`kind create cluster --name=pavantest --config=cluster.yaml`

### Using "requiredDuringSchedulingIgnoredDuringExecution"

`k apply -f affinity.yml`

`k get pods`

![image](https://github.com/user-attachments/assets/df58c914-f7f7-4627-9558-be59f8156230)

 `k describe pod redis-3`

![image](https://github.com/user-attachments/assets/91831ca3-04f8-4943-8237-cf37f4e9d92e)

`k get nodes`

![image](https://github.com/user-attachments/assets/aab807e9-275a-4cc4-be6c-ab1658d702bb)

Applying Labels to the Nodes

`k label node pavantest-worker2 disktype=ssd`

`k get nodes --show-labels`

![image](https://github.com/user-attachments/assets/a8bd9d38-b79e-44ca-b081-8b4a348d5314)

`k get pods -w`

![image](https://github.com/user-attachments/assets/1c392e32-ae29-4a72-9f33-0fbefc5e1913)

`k get pods -o wide`

![image](https://github.com/user-attachments/assets/9166bd52-5e78-422b-bfa4-95718f5d71ad)

---

### Using "preferredDuringSchedulingIgnoredDuringExecution"

`k apply -f affinity2.yml`

`k get pods -w`

![image](https://github.com/user-attachments/assets/2df01471-9b95-4ec0-88f9-5c840e5d5e31)

`k get pods -o wide`

![image](https://github.com/user-attachments/assets/1553251b-4378-4ed0-803c-6ce450453505)

---

### Using "exist" Operator

`k label node pavantest-worker disktype=`

`k apply -f affinity3.yml`

`k get pods -w`

![image](https://github.com/user-attachments/assets/018fdf51-7420-4c4c-a6fc-d08687de34dd)

`k get pods -o wide`

![image](https://github.com/user-attachments/assets/eeb38f56-30ab-4fe7-9ffe-7e32547ee185)

