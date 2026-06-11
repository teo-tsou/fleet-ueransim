# fleet-ueransim

Fleet bundle to deploy UERANSIM (gNB + UE simulator) on K3s clusters.
Depends on free5GC being deployed first.

## Defaults
- MCC: 208 / MNC: 93 (free5GC defaults)
- gNB N2 IP: 10.100.50.250
- gNB N3 IP: 10.100.50.236
- AMF N2 IP: 10.100.50.249
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
