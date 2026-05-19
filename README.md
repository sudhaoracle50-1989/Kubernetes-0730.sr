<div align="center">

<img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png" width="120" alt="Kubernetes Logo"/>

# ☸️ Kubernetes Infrastructure Platform

**Production-grade, multi-cluster Kubernetes orchestration platform**  
built for scale, resilience, and developer velocity.

[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io)
[![Helm](https://img.shields.io/badge/Helm-v3.14-0F1689?style=for-the-badge&logo=helm&logoColor=white)](https://helm.sh)
[![ArgoCD](https://img.shields.io/badge/ArgoCD-v2.10-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)](https://argoproj.github.io)
[![Terraform](https://img.shields.io/badge/Terraform-v1.7-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://terraform.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](./LICENSE)

---

</div>

## 🗺️ Table of Contents

- [📐 Platform Architecture](#-platform-architecture)
- [🌐 Network Architecture](#-network-architecture)
- [🔄 GitOps / CI-CD Pipeline](#-gitops--ci-cd-pipeline)
- [🔒 Security Model](#-security-model)
- [🗂️ Repository Structure](#️-repository-structure)
- [⚡ Quick Start](#-quick-start)
- [🔧 Configuration Reference](#-configuration-reference)
- [📊 Observability Stack](#-observability-stack)
- [🚀 Cluster Environments](#-cluster-environments)
- [🤝 Contributing](#-contributing)

---

## 📐 Platform Architecture

Our platform is built on a **hub-and-spoke** multi-cluster model — a central management cluster orchestrates workloads across dedicated environment clusters.

```svg
<svg width="100%" viewBox="0 0 860 620" xmlns="http://www.w3.org/2000/svg" role="img">
  <title>Kubernetes Platform Architecture</title>
  <desc>Hub-and-spoke multi-cluster architecture with management cluster at center</desc>
  <defs>
    <marker id="arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
    <linearGradient id="hubGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#534AB7"/>
      <stop offset="100%" stop-color="#3C3489"/>
    </linearGradient>
    <linearGradient id="bgGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#1a1a2e"/>
      <stop offset="100%" stop-color="#16213e"/>
    </linearGradient>
    <linearGradient id="prodGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#0F6E56"/>
      <stop offset="100%" stop-color="#085041"/>
    </linearGradient>
    <linearGradient id="stagGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#185FA5"/>
      <stop offset="100%" stop-color="#0C447C"/>
    </linearGradient>
    <linearGradient id="devGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#BA7517"/>
      <stop offset="100%" stop-color="#854F0B"/>
    </linearGradient>
    <linearGradient id="drGrad" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#993C1D"/>
      <stop offset="100%" stop-color="#712B13"/>
    </linearGradient>
  </defs>

  <!-- Background -->
  <rect width="860" height="620" fill="url(#bgGrad)" rx="16"/>

  <!-- Grid dots background -->
  <pattern id="dots" x="0" y="0" width="30" height="30" patternUnits="userSpaceOnUse">
    <circle cx="15" cy="15" r="1" fill="#ffffff" opacity="0.06"/>
  </pattern>
  <rect width="860" height="620" fill="url(#dots)" rx="16"/>

  <!-- Title -->
  <text x="430" y="44" text-anchor="middle" fill="#ffffff" font-size="18" font-weight="700" font-family="monospace" opacity="0.9">☸  KUBERNETES PLATFORM ARCHITECTURE</text>

  <!-- ── HUB CLUSTER (Management) ── -->
  <rect x="280" y="200" width="300" height="220" rx="16" fill="url(#hubGrad)" opacity="0.95" stroke="#AFA9EC" stroke-width="1.5"/>
  <rect x="280" y="200" width="300" height="40" rx="16" fill="#AFA9EC" opacity="0.2"/>
  <text x="430" y="225" text-anchor="middle" fill="#EEEDFE" font-size="13" font-weight="700" font-family="monospace">🛡  MANAGEMENT CLUSTER</text>

  <!-- Hub inner components -->
  <rect x="298" y="252" width="120" height="44" rx="8" fill="#EEEDFE" opacity="0.12" stroke="#AFA9EC" stroke-width="0.8"/>
  <text x="358" y="271" text-anchor="middle" fill="#EEEDFE" font-size="11" font-weight="600" font-family="monospace">ArgoCD</text>
  <text x="358" y="287" text-anchor="middle" fill="#AFA9EC" font-size="10" font-family="monospace">GitOps Engine</text>

  <rect x="442" y="252" width="120" height="44" rx="8" fill="#EEEDFE" opacity="0.12" stroke="#AFA9EC" stroke-width="0.8"/>
  <text x="502" y="271" text-anchor="middle" fill="#EEEDFE" font-size="11" font-weight="600" font-family="monospace">Vault</text>
  <text x="502" y="287" text-anchor="middle" fill="#AFA9EC" font-size="10" font-family="monospace">Secrets Mgmt</text>

  <rect x="298" y="310" width="120" height="44" rx="8" fill="#EEEDFE" opacity="0.12" stroke="#AFA9EC" stroke-width="0.8"/>
  <text x="358" y="329" text-anchor="middle" fill="#EEEDFE" font-size="11" font-weight="600" font-family="monospace">Prometheus</text>
  <text x="358" y="345" text-anchor="middle" fill="#AFA9EC" font-size="10" font-family="monospace">Monitoring</text>

  <rect x="442" y="310" width="120" height="44" rx="8" fill="#EEEDFE" opacity="0.12" stroke="#AFA9EC" stroke-width="0.8"/>
  <text x="502" y="329" text-anchor="middle" fill="#EEEDFE" font-size="11" font-weight="600" font-family="monospace">Grafana</text>
  <text x="502" y="345" text-anchor="middle" fill="#AFA9EC" font-size="10" font-family="monospace">Dashboards</text>

  <rect x="298" y="368" width="264" height="36" rx="8" fill="#EEEDFE" opacity="0.08" stroke="#AFA9EC" stroke-width="0.8"/>
  <text x="430" y="391" text-anchor="middle" fill="#AFA9EC" font-size="10" font-family="monospace">Cluster API  •  RBAC  •  Cert-Manager  •  External-DNS</text>

  <!-- ── PROD CLUSTER ── -->
  <rect x="30" y="80" width="200" height="170" rx="14" fill="url(#prodGrad)" stroke="#5DCAA5" stroke-width="1.5" opacity="0.95"/>
  <text x="130" y="105" text-anchor="middle" fill="#E1F5EE" font-size="12" font-weight="700" font-family="monospace">🟢 PRODUCTION</text>
  <rect x="48" y="118" width="164" height="32" rx="7" fill="#E1F5EE" opacity="0.1" stroke="#5DCAA5" stroke-width="0.7"/>
  <text x="130" y="139" text-anchor="middle" fill="#9FE1CB" font-size="10" font-family="monospace">3× Control Plane</text>
  <rect x="48" y="158" width="164" height="32" rx="7" fill="#E1F5EE" opacity="0.1" stroke="#5DCAA5" stroke-width="0.7"/>
  <text x="130" y="179" text-anchor="middle" fill="#9FE1CB" font-size="10" font-family="monospace">12× Worker Nodes</text>
  <rect x="48" y="198" width="164" height="32" rx="7" fill="#E1F5EE" opacity="0.1" stroke="#5DCAA5" stroke-width="0.7"/>
  <text x="130" y="219" text-anchor="middle" fill="#9FE1CB" font-size="10" font-family="monospace">Istio Service Mesh</text>

  <!-- ── STAGING CLUSTER ── -->
  <rect x="630" y="80" width="200" height="170" rx="14" fill="url(#stagGrad)" stroke="#85B7EB" stroke-width="1.5" opacity="0.95"/>
  <text x="730" y="105" text-anchor="middle" fill="#E6F1FB" font-size="12" font-weight="700" font-family="monospace">🔵 STAGING</text>
  <rect x="648" y="118" width="164" height="32" rx="7" fill="#E6F1FB" opacity="0.1" stroke="#85B7EB" stroke-width="0.7"/>
  <text x="730" y="139" text-anchor="middle" fill="#B5D4F4" font-size="10" font-family="monospace">1× Control Plane</text>
  <rect x="648" y="158" width="164" height="32" rx="7" fill="#E6F1FB" opacity="0.1" stroke="#85B7EB" stroke-width="0.7"/>
  <text x="730" y="179" text-anchor="middle" fill="#B5D4F4" font-size="10" font-family="monospace">4× Worker Nodes</text>
  <rect x="648" y="198" width="164" height="32" rx="7" fill="#E6F1FB" opacity="0.1" stroke="#85B7EB" stroke-width="0.7"/>
  <text x="730" y="219" text-anchor="middle" fill="#B5D4F4" font-size="10" font-family="monospace">Linkerd Mesh</text>

  <!-- ── DEV CLUSTER ── -->
  <rect x="30" y="400" width="200" height="170" rx="14" fill="url(#devGrad)" stroke="#EF9F27" stroke-width="1.5" opacity="0.95"/>
  <text x="130" y="425" text-anchor="middle" fill="#FAEEDA" font-size="12" font-weight="700" font-family="monospace">🟡 DEVELOPMENT</text>
  <rect x="48" y="438" width="164" height="32" rx="7" fill="#FAEEDA" opacity="0.1" stroke="#EF9F27" stroke-width="0.7"/>
  <text x="130" y="459" text-anchor="middle" fill="#FAC775" font-size="10" font-family="monospace">1× Control Plane</text>
  <rect x="48" y="478" width="164" height="32" rx="7" fill="#FAEEDA" opacity="0.1" stroke="#EF9F27" stroke-width="0.7"/>
  <text x="130" y="499" text-anchor="middle" fill="#FAC775" font-size="10" font-family="monospace">3× Worker Nodes</text>
  <rect x="48" y="518" width="164" height="32" rx="7" fill="#FAEEDA" opacity="0.1" stroke="#EF9F27" stroke-width="0.7"/>
  <text x="130" y="539" text-anchor="middle" fill="#FAC775" font-size="10" font-family="monospace">Minimal Addons</text>

  <!-- ── DR CLUSTER ── -->
  <rect x="630" y="400" width="200" height="170" rx="14" fill="url(#drGrad)" stroke="#F0997B" stroke-width="1.5" opacity="0.95"/>
  <text x="730" y="425" text-anchor="middle" fill="#FAECE7" font-size="12" font-weight="700" font-family="monospace">🔴 DR / FAILOVER</text>
  <rect x="648" y="438" width="164" height="32" rx="7" fill="#FAECE7" opacity="0.1" stroke="#F0997B" stroke-width="0.7"/>
  <text x="730" y="459" text-anchor="middle" fill="#F5C4B3" font-size="10" font-family="monospace">3× Control Plane</text>
  <rect x="648" y="478" width="164" height="32" rx="7" fill="#FAECE7" opacity="0.1" stroke="#F0997B" stroke-width="0.7"/>
  <text x="730" y="499" text-anchor="middle" fill="#F5C4B3" font-size="10" font-family="monospace">8× Worker Nodes</text>
  <rect x="648" y="518" width="164" height="32" rx="7" fill="#FAECE7" opacity="0.1" stroke="#F0997B" stroke-width="0.7"/>
  <text x="730" y="539" text-anchor="middle" fill="#F5C4B3" font-size="10" font-family="monospace">Velero Backup</text>

  <!-- Connector Lines: Hub ↔ Clusters -->
  <line x1="280" y1="295" x2="230" y2="200" stroke="#AFA9EC" stroke-width="1.5" stroke-dasharray="6 3" opacity="0.7" marker-end="url(#arr)"/>
  <line x1="580" y1="295" x2="630" y2="200" stroke="#AFA9EC" stroke-width="1.5" stroke-dasharray="6 3" opacity="0.7" marker-end="url(#arr)"/>
  <line x1="290" y1="400" x2="230" y2="400" stroke="#AFA9EC" stroke-width="1.5" stroke-dasharray="6 3" opacity="0.7" marker-end="url(#arr)"/>
  <line x1="570" y1="400" x2="630" y2="470" stroke="#AFA9EC" stroke-width="1.5" stroke-dasharray="6 3" opacity="0.7" marker-end="url(#arr)"/>

  <!-- Legend -->
  <rect x="220" y="570" width="420" height="34" rx="8" fill="#ffffff" opacity="0.06"/>
  <circle cx="244" cy="587" r="5" fill="#5DCAA5"/>
  <text x="255" y="591" fill="#9FE1CB" font-size="10" font-family="monospace">Production</text>
  <circle cx="336" cy="587" r="5" fill="#85B7EB"/>
  <text x="347" y="591" fill="#B5D4F4" font-size="10" font-family="monospace">Staging</text>
  <circle cx="414" cy="587" r="5" fill="#EF9F27"/>
  <text x="425" y="591" fill="#FAC775" font-size="10" font-family="monospace">Development</text>
  <circle cx="514" cy="587" r="5" fill="#F0997B"/>
  <text x="525" y="591" fill="#F5C4B3" font-size="10" font-family="monospace">DR / Failover</text>
  <line x1="590" y1="587" x2="618" y2="587" stroke="#AFA9EC" stroke-width="1.5" stroke-dasharray="4 2"/>
  <text x="624" y="591" fill="#AFA9EC" font-size="10" font-family="monospace">GitOps sync</text>
</svg>
```

---

## 🌐 Network Architecture

Traffic enters through a cloud load balancer, hits NGINX Ingress, passes through the service mesh, and reaches your pods — all with mTLS between every hop.

```svg
<svg width="100%" viewBox="0 0 860 380" xmlns="http://www.w3.org/2000/svg">
  <title>Kubernetes Network Architecture</title>
  <defs>
    <marker id="arr2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
    <linearGradient id="netBg" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#042C53"/>
      <stop offset="100%" stop-color="#0C447C"/>
    </linearGradient>
  </defs>

  <rect width="860" height="380" fill="url(#netBg)" rx="14"/>
  <pattern id="grid" x="0" y="0" width="40" height="40" patternUnits="userSpaceOnUse">
    <path d="M40 0L0 0L0 40" fill="none" stroke="#ffffff" stroke-width="0.3" opacity="0.08"/>
  </pattern>
  <rect width="860" height="380" fill="url(#grid)" rx="14"/>

  <!-- Title -->
  <text x="430" y="36" text-anchor="middle" fill="#E6F1FB" font-size="15" font-weight="700" font-family="monospace">🌐  NETWORK TRAFFIC FLOW</text>

  <!-- Internet -->
  <rect x="30" y="150" width="100" height="70" rx="12" fill="#185FA5" opacity="0.8" stroke="#85B7EB" stroke-width="1.5"/>
  <text x="80" y="180" text-anchor="middle" fill="#E6F1FB" font-size="20" font-family="monospace">🌍</text>
  <text x="80" y="200" text-anchor="middle" fill="#B5D4F4" font-size="11" font-family="monospace">Internet</text>
  <text x="80" y="214" text-anchor="middle" fill="#85B7EB" font-size="9" font-family="monospace">Users / APIs</text>

  <!-- Cloud LB -->
  <rect x="168" y="145" width="108" height="80" rx="12" fill="#0F6E56" opacity="0.85" stroke="#5DCAA5" stroke-width="1.5"/>
  <text x="222" y="175" text-anchor="middle" fill="#E1F5EE" font-size="18" font-family="monospace">⚖️</text>
  <text x="222" y="196" text-anchor="middle" fill="#9FE1CB" font-size="11" font-weight="700" font-family="monospace">Cloud LB</text>
  <text x="222" y="212" text-anchor="middle" fill="#5DCAA5" font-size="9" font-family="monospace">L4 / L7 NLB</text>

  <!-- Ingress -->
  <rect x="312" y="130" width="118" height="110" rx="12" fill="#534AB7" opacity="0.9" stroke="#AFA9EC" stroke-width="1.5"/>
  <text x="371" y="162" text-anchor="middle" fill="#EEEDFE" font-size="17" font-family="monospace">🔀</text>
  <text x="371" y="183" text-anchor="middle" fill="#EEEDFE" font-size="11" font-weight="700" font-family="monospace">NGINX Ingress</text>
  <text x="371" y="199" text-anchor="middle" fill="#AFA9EC" font-size="9" font-family="monospace">TLS termination</text>
  <text x="371" y="213" text-anchor="middle" fill="#AFA9EC" font-size="9" font-family="monospace">Rate limiting</text>
  <text x="371" y="227" text-anchor="middle" fill="#AFA9EC" font-size="9" font-family="monospace">WAF rules</text>

  <!-- Service Mesh -->
  <rect x="466" y="130" width="118" height="110" rx="12" fill="#993C1D" opacity="0.85" stroke="#F0997B" stroke-width="1.5"/>
  <text x="525" y="162" text-anchor="middle" fill="#FAECE7" font-size="17" font-family="monospace">🕸</text>
  <text x="525" y="183" text-anchor="middle" fill="#FAECE7" font-size="11" font-weight="700" font-family="monospace">Service Mesh</text>
  <text x="525" y="199" text-anchor="middle" fill="#F5C4B3" font-size="9" font-family="monospace">mTLS between pods</text>
  <text x="525" y="213" text-anchor="middle" fill="#F5C4B3" font-size="9" font-family="monospace">Traffic policies</text>
  <text x="525" y="227" text-anchor="middle" fill="#F5C4B3" font-size="9" font-family="monospace">Observability</text>

  <!-- Services -->
  <rect x="622" y="80" width="100" height="58" rx="10" fill="#BA7517" opacity="0.85" stroke="#EF9F27" stroke-width="1.2"/>
  <text x="672" y="107" text-anchor="middle" fill="#FAEEDA" font-size="11" font-weight="700" font-family="monospace">Service A</text>
  <text x="672" y="122" text-anchor="middle" fill="#FAC775" font-size="9" font-family="monospace">ClusterIP • 3 pods</text>

  <rect x="622" y="158" width="100" height="58" rx="10" fill="#BA7517" opacity="0.85" stroke="#EF9F27" stroke-width="1.2"/>
  <text x="672" y="185" text-anchor="middle" fill="#FAEEDA" font-size="11" font-weight="700" font-family="monospace">Service B</text>
  <text x="672" y="200" text-anchor="middle" fill="#FAC775" font-size="9" font-family="monospace">ClusterIP • 2 pods</text>

  <rect x="622" y="236" width="100" height="58" rx="10" fill="#BA7517" opacity="0.85" stroke="#EF9F27" stroke-width="1.2"/>
  <text x="672" y="263" text-anchor="middle" fill="#FAEEDA" font-size="11" font-weight="700" font-family="monospace">Service C</text>
  <text x="672" y="278" text-anchor="middle" fill="#FAC775" font-size="9" font-family="monospace">ClusterIP • 5 pods</text>

  <!-- Arrows -->
  <line x1="130" y1="185" x2="166" y2="185" stroke="#5DCAA5" stroke-width="2" marker-end="url(#arr2)"/>
  <line x1="276" y1="185" x2="310" y2="185" stroke="#AFA9EC" stroke-width="2" marker-end="url(#arr2)"/>
  <line x1="430" y1="185" x2="464" y2="185" stroke="#F0997B" stroke-width="2" marker-end="url(#arr2)"/>

  <line x1="584" y1="155" x2="620" y2="115" stroke="#EF9F27" stroke-width="1.5" marker-end="url(#arr2)"/>
  <line x1="584" y1="185" x2="620" y2="187" stroke="#EF9F27" stroke-width="1.5" marker-end="url(#arr2)"/>
  <line x1="584" y1="215" x2="620" y2="255" stroke="#EF9F27" stroke-width="1.5" marker-end="url(#arr2)"/>

  <!-- Port labels on arrows -->
  <rect x="136" y="172" width="28" height="14" rx="3" fill="#0C447C"/>
  <text x="150" y="183" text-anchor="middle" fill="#85B7EB" font-size="8" font-family="monospace">:443</text>
  <rect x="282" y="172" width="28" height="14" rx="3" fill="#26215C"/>
  <text x="296" y="183" text-anchor="middle" fill="#AFA9EC" font-size="8" font-family="monospace">HTTP</text>

  <!-- mTLS badge -->
  <rect x="435" y="172" width="32" height="14" rx="3" fill="#712B13"/>
  <text x="451" y="183" text-anchor="middle" fill="#F5C4B3" font-size="8" font-family="monospace">mTLS</text>

  <!-- Legend bottom -->
  <rect x="200" y="340" width="460" height="26" rx="6" fill="#ffffff" opacity="0.06"/>
  <text x="246" y="357" fill="#9FE1CB" font-size="9" font-family="monospace">🟢 Encrypted external</text>
  <text x="380" y="357" fill="#AFA9EC" font-size="9" font-family="monospace">🔵 Internal routing</text>
  <text x="498" y="357" fill="#F5C4B3" font-size="9" font-family="monospace">🔴 mTLS mesh</text>
  <text x="590" y="357" fill="#FAC775" font-size="9" font-family="monospace">🟡 Services</text>
</svg>
```

---

## 🔄 GitOps / CI-CD Pipeline

Every commit triggers a pipeline that builds, tests, scans, and automatically syncs to the target cluster via ArgoCD.

```svg
<svg width="100%" viewBox="0 0 860 200" xmlns="http://www.w3.org/2000/svg">
  <title>GitOps CI/CD Pipeline</title>
  <defs>
    <marker id="arr3" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
    <linearGradient id="pipelineBg" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#17113a"/>
      <stop offset="100%" stop-color="#0d2137"/>
    </linearGradient>
  </defs>

  <rect width="860" height="200" fill="url(#pipelineBg)" rx="14"/>

  <!-- Stage boxes -->
  <!-- 1. Code Push -->
  <rect x="20" y="60" width="108" height="80" rx="10" fill="#3C3489" opacity="0.9" stroke="#7F77DD" stroke-width="1.3"/>
  <text x="74" y="90" text-anchor="middle" fill="#EEEDFE" font-size="18">📝</text>
  <text x="74" y="108" text-anchor="middle" fill="#EEEDFE" font-size="10" font-weight="700" font-family="monospace">Code Push</text>
  <text x="74" y="122" text-anchor="middle" fill="#AFA9EC" font-size="9" font-family="monospace">GitHub / GitLab</text>

  <!-- 2. CI Build -->
  <rect x="150" y="60" width="108" height="80" rx="10" fill="#185FA5" opacity="0.9" stroke="#85B7EB" stroke-width="1.3"/>
  <text x="204" y="90" text-anchor="middle" fill="#E6F1FB" font-size="18">🏗</text>
  <text x="204" y="108" text-anchor="middle" fill="#E6F1FB" font-size="10" font-weight="700" font-family="monospace">CI Build</text>
  <text x="204" y="122" text-anchor="middle" fill="#B5D4F4" font-size="9" font-family="monospace">Docker · Kaniko</text>

  <!-- 3. Tests -->
  <rect x="280" y="60" width="108" height="80" rx="10" fill="#0F6E56" opacity="0.9" stroke="#5DCAA5" stroke-width="1.3"/>
  <text x="334" y="90" text-anchor="middle" fill="#E1F5EE" font-size="18">🧪</text>
  <text x="334" y="108" text-anchor="middle" fill="#E1F5EE" font-size="10" font-weight="700" font-family="monospace">Test Suite</text>
  <text x="334" y="122" text-anchor="middle" fill="#9FE1CB" font-size="9" font-family="monospace">Unit · E2E · Lint</text>

  <!-- 4. Security Scan -->
  <rect x="410" y="60" width="108" height="80" rx="10" fill="#993C1D" opacity="0.9" stroke="#F0997B" stroke-width="1.3"/>
  <text x="464" y="90" text-anchor="middle" fill="#FAECE7" font-size="18">🔍</text>
  <text x="464" y="108" text-anchor="middle" fill="#FAECE7" font-size="10" font-weight="700" font-family="monospace">Security Scan</text>
  <text x="464" y="122" text-anchor="middle" fill="#F5C4B3" font-size="9" font-family="monospace">Trivy · Snyk</text>

  <!-- 5. Registry Push -->
  <rect x="540" y="60" width="108" height="80" rx="10" fill="#BA7517" opacity="0.9" stroke="#EF9F27" stroke-width="1.3"/>
  <text x="594" y="90" text-anchor="middle" fill="#FAEEDA" font-size="18">📦</text>
  <text x="594" y="108" text-anchor="middle" fill="#FAEEDA" font-size="10" font-weight="700" font-family="monospace">Push Image</text>
  <text x="594" y="122" text-anchor="middle" fill="#FAC775" font-size="9" font-family="monospace">ECR / GCR / GHCR</text>

  <!-- 6. GitOps Sync -->
  <rect x="670" y="60" width="108" height="80" rx="10" fill="#3B6D11" opacity="0.9" stroke="#97C459" stroke-width="1.3"/>
  <text x="724" y="90" text-anchor="middle" fill="#EAF3DE" font-size="18">♻️</text>
  <text x="724" y="108" text-anchor="middle" fill="#EAF3DE" font-size="10" font-weight="700" font-family="monospace">ArgoCD Sync</text>
  <text x="724" y="122" text-anchor="middle" fill="#C0DD97" font-size="9" font-family="monospace">Auto-deploy</text>

  <!-- 7. Deploy badge -->
  <rect x="800" y="83" width="44" height="34" rx="17" fill="#639922" opacity="0.9" stroke="#97C459" stroke-width="1.5"/>
  <text x="822" y="105" text-anchor="middle" fill="#EAF3DE" font-size="11">✅</text>

  <!-- Arrows between stages -->
  <line x1="128" y1="100" x2="148" y2="100" stroke="#7F77DD" stroke-width="2" marker-end="url(#arr3)"/>
  <line x1="258" y1="100" x2="278" y2="100" stroke="#85B7EB" stroke-width="2" marker-end="url(#arr3)"/>
  <line x1="388" y1="100" x2="408" y2="100" stroke="#5DCAA5" stroke-width="2" marker-end="url(#arr3)"/>
  <line x1="518" y1="100" x2="538" y2="100" stroke="#F0997B" stroke-width="2" marker-end="url(#arr3)"/>
  <line x1="648" y1="100" x2="668" y2="100" stroke="#EF9F27" stroke-width="2" marker-end="url(#arr3)"/>
  <line x1="778" y1="100" x2="798" y2="100" stroke="#97C459" stroke-width="2" marker-end="url(#arr3)"/>

  <!-- Stage step numbers -->
  <circle cx="74"  cy="62" r="9" fill="#7F77DD"/>
  <text x="74"  y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">1</text>
  <circle cx="204" cy="62" r="9" fill="#85B7EB"/>
  <text x="204" y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">2</text>
  <circle cx="334" cy="62" r="9" fill="#5DCAA5"/>
  <text x="334" y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">3</text>
  <circle cx="464" cy="62" r="9" fill="#F0997B"/>
  <text x="464" y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">4</text>
  <circle cx="594" cy="62" r="9" fill="#EF9F27"/>
  <text x="594" y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">5</text>
  <circle cx="724" cy="62" r="9" fill="#97C459"/>
  <text x="724" y="66" text-anchor="middle" fill="white" font-size="9" font-weight="700" font-family="monospace">6</text>

  <!-- Bottom hint -->
  <text x="430" y="178" text-anchor="middle" fill="#ffffff" font-size="10" font-family="monospace" opacity="0.5">Average pipeline time: ~4 min  •  Rollback: instant via ArgoCD</text>
</svg>
```

---

## 🗂️ Repository Structure

```
k8s-platform/
├── 📁 clusters/                    # Cluster-level configurations
│   ├── production/
│   │   ├── cluster-config.yaml
│   │   └── addons/
│   ├── staging/
│   ├── development/
│   └── dr/
│
├── 📁 apps/                        # Application manifests (ArgoCD AppSets)
│   ├── _templates/                 # Reusable Helm chart wrappers
│   ├── api-gateway/
│   ├── auth-service/
│   └── ...
│
├── 📁 platform/                    # Platform services
│   ├── cert-manager/               # TLS certificate automation
│   ├── external-dns/               # Automatic DNS management
│   ├── ingress-nginx/              # Ingress controller
│   ├── kyverno/                    # Policy engine
│   ├── prometheus-stack/           # Monitoring (Prometheus + Grafana)
│   ├── velero/                     # Backup & DR
│   └── vault/                      # Secrets management
│
├── 📁 gitops/                      # ArgoCD configurations
│   ├── applicationsets/            # Multi-cluster ApplicationSets
│   ├── projects/                   # ArgoCD project definitions
│   └── repositories/               # Repository credentials
│
├── 📁 terraform/                   # Cloud infrastructure
│   ├── modules/
│   │   ├── eks/                    # AWS EKS module
│   │   ├── gke/                    # GKE module
│   │   └── networking/
│   └── environments/
│
├── 📁 scripts/                     # Operational scripts
│   ├── bootstrap-cluster.sh
│   ├── rotate-secrets.sh
│   └── dr-failover.sh
│
└── 📁 docs/                        # Documentation
    ├── runbooks/
    ├── architecture/
    └── onboarding/
```

---

## ⚡ Quick Start

### Prerequisites

| Tool | Version | Purpose |
|------|---------|---------|
| `kubectl` | ≥ 1.29 | Cluster interaction |
| `helm` | ≥ 3.14 | Chart deployment |
| `argocd` CLI | ≥ 2.10 | GitOps management |
| `terraform` | ≥ 1.7 | Infrastructure provisioning |
| `vault` CLI | ≥ 1.15 | Secrets management |

### Bootstrap a New Cluster

```bash
# 1. Clone this repository
git clone https://github.com/your-org/k8s-platform.git
cd k8s-platform

# 2. Provision infrastructure with Terraform
cd terraform/environments/production
terraform init && terraform apply

# 3. Bootstrap the cluster (installs ArgoCD + platform addons)
./scripts/bootstrap-cluster.sh \
  --cluster production \
  --region us-east-1

# 4. Verify all platform components are healthy
kubectl get pods -A | grep -v Running

# 5. Access ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 8080:443
# open https://localhost:8080
```

### Deploy an Application

```bash
# Create an ArgoCD Application pointing to your app manifests
kubectl apply -f - <<EOF
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-service
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/your-org/k8s-platform
    targetRevision: HEAD
    path: apps/my-service
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
EOF
```

---

## 🔒 Security Model

```svg
<svg width="100%" viewBox="0 0 860 280" xmlns="http://www.w3.org/2000/svg">
  <title>Security Layers</title>
  <defs>
    <linearGradient id="secBg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#1a0a2e"/>
      <stop offset="100%" stop-color="#2a0d1a"/>
    </linearGradient>
  </defs>
  <rect width="860" height="280" fill="url(#secBg)" rx="14"/>

  <text x="430" y="36" text-anchor="middle" fill="#F4C0D1" font-size="15" font-weight="700" font-family="monospace">🔒  SECURITY LAYERS</text>

  <!-- Layer: Cloud/Perimeter -->
  <rect x="40" y="55" width="780" height="44" rx="10" fill="#993556" opacity="0.3" stroke="#D4537E" stroke-width="1.2" stroke-dasharray="5 3"/>
  <text x="60" y="80" fill="#F4C0D1" font-size="11" font-weight="700" font-family="monospace">LAYER 1 · PERIMETER</text>
  <text x="290" y="80" fill="#ED93B1" font-size="10" font-family="monospace">WAF  •  DDoS Protection  •  CloudFront / Cloudflare  •  IP Allowlist</text>

  <!-- Layer: Cluster -->
  <rect x="60" y="108" width="740" height="44" rx="10" fill="#534AB7" opacity="0.3" stroke="#AFA9EC" stroke-width="1.2" stroke-dasharray="5 3"/>
  <text x="80" y="133" fill="#EEEDFE" font-size="11" font-weight="700" font-family="monospace">LAYER 2 · CLUSTER</text>
  <text x="280" y="133" fill="#AFA9EC" font-size="10" font-family="monospace">RBAC  •  PodSecurity Standards  •  Kyverno Policies  •  Admission Webhooks</text>

  <!-- Layer: Network -->
  <rect x="80" y="161" width="700" height="44" rx="10" fill="#185FA5" opacity="0.3" stroke="#85B7EB" stroke-width="1.2" stroke-dasharray="5 3"/>
  <text x="100" y="186" fill="#E6F1FB" font-size="11" font-weight="700" font-family="monospace">LAYER 3 · NETWORK</text>
  <text x="290" y="186" fill="#B5D4F4" font-size="10" font-family="monospace">Network Policies  •  mTLS  •  Cilium eBPF  •  DNS Filtering</text>

  <!-- Layer: Workload -->
  <rect x="100" y="214" width="660" height="44" rx="10" fill="#0F6E56" opacity="0.3" stroke="#5DCAA5" stroke-width="1.2" stroke-dasharray="5 3"/>
  <text x="120" y="239" fill="#E1F5EE" font-size="11" font-weight="700" font-family="monospace">LAYER 4 · WORKLOAD</text>
  <text x="300" y="239" fill="#9FE1CB" font-size="10" font-family="monospace">Non-root containers  •  Read-only FS  •  Trivy scanning  •  SBOM</text>

  <!-- Shield icon at center-right -->
  <text x="800" y="160" text-anchor="middle" fill="#D4537E" font-size="56" opacity="0.2">🛡</text>
</svg>
```

**Key Security Controls:**

| Control | Tool | Status |
|---------|------|--------|
| Secrets management | HashiCorp Vault + ESO | ✅ Active |
| Image scanning | Trivy (in CI + admission) | ✅ Active |
| Policy enforcement | Kyverno | ✅ Active |
| mTLS everywhere | Istio / Linkerd | ✅ Active |
| Audit logging | Falco + CloudTrail | ✅ Active |
| RBAC | OPA Gatekeeper | ✅ Active |

---

## 📊 Observability Stack

The full observability platform is deployed via the `platform/prometheus-stack` Helm chart.

```
Metrics ──→ Prometheus ──→ Grafana (dashboards)
                 │
                 └──→ AlertManager ──→ PagerDuty / Slack

Logs ────→ Fluent Bit ──→ Loki ──→ Grafana

Traces ──→ OpenTelemetry Collector ──→ Tempo ──→ Grafana
```

### Key Dashboards

| Dashboard | Description | Link |
|-----------|-------------|------|
| Cluster Overview | Node health, resource pressure | `d/cluster-overview` |
| Workload Health | Pod restarts, OOMKills | `d/workload-health` |
| Network Traffic | Ingress throughput, error rates | `d/network-traffic` |
| Cost Allocation | FinOps per-namespace spend | `d/cost-allocation` |
| SLO Tracker | Error budgets across services | `d/slo-tracker` |

---

## 🚀 Cluster Environments

| Cluster | Cloud | Region | Nodes | K8s Version | Purpose |
|---------|-------|--------|-------|-------------|---------|
| `prod-us-east` | AWS EKS | us-east-1 | 12× m6i.2xl | v1.29 | Primary production |
| `prod-eu-west` | AWS EKS | eu-west-1 | 10× m6i.2xl | v1.29 | EU production |
| `staging` | GKE | us-central1 | 4× n2-std-8 | v1.29 | Pre-prod validation |
| `development` | GKE | us-central1 | 3× n2-std-4 | v1.29 | Feature development |
| `dr-failover` | AWS EKS | us-west-2 | 8× m6i.2xl | v1.29 | Disaster recovery |
| `management` | AWS EKS | us-east-1 | 3× m6i.xl | v1.29 | Hub / control plane |

---

## 🔧 Configuration Reference

### Helm Values Override Pattern

```yaml
# clusters/production/apps/my-service/values-override.yaml
replicaCount: 3

resources:
  requests:
    cpu: "500m"
    memory: "512Mi"
  limits:
    cpu: "2000m"
    memory: "2Gi"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 20
  targetCPUUtilizationPercentage: 70

podDisruptionBudget:
  enabled: true
  minAvailable: 2

topologySpreadConstraints:
  - maxSkew: 1
    topologyKey: topology.kubernetes.io/zone
    whenUnsatisfiable: DoNotSchedule
```

### Kyverno Policy Example

```yaml
# platform/kyverno/policies/require-resource-limits.yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-resource-limits
spec:
  validationFailureAction: Enforce
  rules:
    - name: check-container-resources
      match:
        resources:
          kinds: [Pod]
      validate:
        message: "Resource limits are required for all containers."
        pattern:
          spec:
            containers:
              - resources:
                  limits:
                    memory: "?*"
                    cpu: "?*"
```

---

## 🤝 Contributing

1. **Fork** the repository and create a feature branch: `git checkout -b feat/my-change`
2. **Follow** the GitOps flow — all changes go through PRs, no direct commits to `main`
3. **Validate** your manifests: `kubectl apply --dry-run=client -f .`
4. **Test** in the `development` cluster before raising a PR to `staging`
5. **Document** runbooks for any new platform component in `docs/runbooks/`

### Branch Strategy

```
main ──────────────────────────────────────────→ production
  └── staging ──────────────────────────────→ staging cluster
        └── feat/* ──────────────────→ development cluster
```

---

<div align="center">

**Maintained by the Platform Engineering team**

[![Slack](https://img.shields.io/badge/Slack-%23platform--eng-4A154B?style=flat&logo=slack)](https://slack.com)
[![Confluence](https://img.shields.io/badge/Docs-Confluence-0052CC?style=flat&logo=confluence)](https://confluence.example.com)
[![PagerDuty](https://img.shields.io/badge/On--call-PagerDuty-06AC38?style=flat&logo=pagerduty)](https://pagerduty.com)

*For incidents, page `#platform-oncall` · For questions, use `#platform-eng` on Slack*

</div>
