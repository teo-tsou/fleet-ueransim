# fleet-ueransim

Fleet bundle to deploy UERANSIM (gNB + UE simulator) on K3s clusters.

Configured for the **edge-UPF split topology**: deploy this to the EDGE UC
alongside `fleet-free5gc-upf`; the control plane (`fleet-free5gc-control`) runs
on the core UC. Deploy order: **UPF → control → ueransim**, with the IntEdge
`amf` binding wired from this UC to the core.

## Topology
- **N2 (gNB→AMF)** crosses UCs → pod network via the IntEdge `amf` binding
  (`amf.upstream.svc.cluster.local:38412`). `n2network` (Multus) OFF.
- **N3 (gNB→UPF)** is LOCAL → the edge UPF is in this same UC. gNB GTP-U on the
  local N3 ipvlan (`10.100.50.234/29`), reaching the UPF's N3 (`10.100.50.233`).
  `n3network` (Multus) ON.

## Defaults
- MCC: 208 / MNC: 93 (free5GC defaults)
- gNB N3 IP: 10.100.50.234 (local ipvlan; UPF N3 is .233)
- AMF N2: `amf.upstream.svc.cluster.local:38412` (IntEdge `amf` binding)
- UE IMSI: 208930000000003

## Add to Fleet
Rancher → Fleet → Git Repos → Add Repository:

| Field          | Value                                          |
|----------------|------------------------------------------------|
| Name           | `fleet-ueransim`                               |
| Repository URL | `https://github.com/teo-tsou/fleet-ueransim`   |
| Branch         | `main`                                         |
| Paths          | `/`                                            |

## Verify
```bash
kubectl get pods -n ueransim
kubectl logs -n ueransim -l app=gnb
kubectl logs -n ueransim -l app=ue
```
