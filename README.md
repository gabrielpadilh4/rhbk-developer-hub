# Red Hat Developer Hub (RHDH) Integration with Red Hat build of Keycloak (RHBK)

This repository provides a complete guide and the necessary manifests to install and integrate the Red Hat Developer Hub with Red Hat build of Keycloak on OpenShift. 

## 🏗️ Architecture Overview
The integration consists of two main parts:
1. **Red Hat build of Keycloak (RHBK):** Acts as the Identity Provider (IdP) for authentication and the source of truth for Users.
2. **Red Hat Developer Hub (RHDH):** The developer portal that uses Keycloak for OIDC authentication and synchronizes its software catalog with Keycloak's organizational data.

## 📁 Repository Structure
* **`rhbk/`**: Contains manifests for deploying Keycloak, its PostgreSQL database, and the `rhdh` realm configuration.
* **`developer-hub/`**: Contains the RHDH Custom Resource, dynamic plugin configurations, and app-level settings.

## 🛠️ Prerequisites
* The `oc` CLI tool authenticated to your cluster.
* `openssl` for local certificate generation.

## 🚀 Deployment Roadmap

To achieve a successful integration, follow the documentation in each subdirectory in this specific order:

### Phase 1: Install Red Hat build of Keycloak and Red Hat Developer Hub Operators
1. Create the necessary resources for the Red Hat build of Keycloak and Red Hat Developer Hub Operators
   ```
   oc apply -f operators.yaml
   ```
2. Approve the install plan for Red Hat build of Keycloak
   ```   
   oc get installplan -n rhbk
   oc patch installplan/<install-plan-name> -n rhbk --type merge --patch '{"spec":{"approved":true}}'
   ```
   **NOTE:** Sometimes take some few minutes to the install plan appear due the OLM activies like downloading images and more.
3. Approve the install plan for Red Hat Developer Hub
   ```   
   oc get installplan -n rhdh-operator
   oc patch installplan/<install-plan-name> -n rhdh-operator --type merge --patch '{"spec":{"approved":true}}'
   ```
   The result is the Red Hat build of Keycloak installed in the `rhbk` namespace and the Red Hat Developer Hub installed in the `rhdh-operator` namespace.
      ![Operator Installed](./operators.png)

### Phase 2: Identity Provider Setup
Navigate to the [RHBK directory](./rhbk/README.md) to:
1. Deploy a PostgreSQL database for Keycloak.
2. Creatte the Red Hat build of Keycloak instance

### Phase 3: Developer Hub Setup
Once Keycloak is running and configured, navigate to the [Developer Hub directory](./developer-hub/README.md) to:
1. Configure `app-config.yaml` with the OIDC endpoints and secrets generated in Phase 1.
2. Enable the Keycloak dynamic plugin via ConfigMap.
3. Deploy the RHDH instance via the Backstage Custom Resource.
4. Import the pre-configured `rhbk/rhdh-realm.yaml` and set up the `rhdh` realm and client.
