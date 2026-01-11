<div align="center">

# 🛣️ BlackRoad OS Integration

<p align="center">
  <a href="https://blackroad.io"><img src="https://img.shields.io/badge/BlackRoad-OS-000000?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iI0Y1QTYyMyIgZD0iTTEyIDJMMyAyMGgxOEwxMiAyeiIvPjwvc3ZnPg==" alt="BlackRoad OS"></a>
  <a href="https://github.com/BlackRoad-OS"><img src="https://img.shields.io/badge/Org-BlackRoad--OS-F5A623?style=for-the-badge" alt="BlackRoad-OS"></a>
  <a href="https://blackroad.io"><img src="https://img.shields.io/badge/Website-blackroad.io-FF1D6C?style=for-the-badge" alt="Website"></a>
</p>

<p align="center">
  <strong>Part of the BlackRoad OS Infrastructure</strong><br>
  <em>Building the future of decentralized systems</em>
</p>

<p align="center">
  <img src="https://img.shields.io/github/license/BlackRoad-OS/harness?style=flat-square&color=9C27B0" alt="License">
  <img src="https://img.shields.io/github/stars/BlackRoad-OS/harness?style=flat-square&color=F5A623" alt="Stars">
  <img src="https://img.shields.io/github/forks/BlackRoad-OS/harness?style=flat-square&color=2979FF" alt="Forks">
</p>

---

</div>

# Harness
Harness Open Source is an open source development platform packed with the power of code hosting, automated DevOps pipelines, hosted development environments (Gitspaces), and artifact registries.

## Overview
Harness Open source is an open source development platform packed with the power of code hosting, automated DevOps pipelines, Gitspaces, and artifact registries.


## Running Harness locally
> The latest publicly released docker image can be found on [harness/harness](https://hub.docker.com/r/harness/harness).

To install Harness yourself, simply run the command below. Once the container is up, you can visit http://localhost:3000 in your browser.

```bash
docker run -d \
  -p 3000:3000 \
  -p 3022:3022 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /tmp/harness:/data \
  --name harness \
  --restart always \
  harness/harness
```
> The Harness image uses a volume to store the database and repositories. It is highly recommended to use a bind mount or named volume as otherwise all data will be lost once the container is stopped.

See [developer.harness.io](https://developer.harness.io/docs/open-source) to learn how to get the most out of Harness.

## Where is Drone?

Harness Open Source represents a massive investment in the next generation of Drone. Where Drone focused solely on continuous integration, Harness adds source code hosting, developer environments (gitspaces), and artifact registries; providing teams with an end-to-end, open source DevOps platform.

The goal is for Harness to eventually be at full parity with Drone in terms of pipeline capabilities, allowing users to seamlessly migrate from Drone to Harness.

But, we expect this to take some time, which is why we took a snapshot of Drone as a feature branch [drone](https://github.com/harness/harness/tree/drone) ([README](https://github.com/harness/harness/blob/drone/.github/readme.md)) so it can continue development.

As for Harness, the development is taking place on the [main](https://github.com/harness/harness/tree/main) branch.

For more information on Harness, please visit [developer.harness.io](https://developer.harness.io/).

For more information on Drone, please visit [drone.io](https://www.drone.io/).

## Harness Open Source Development
### Pre-Requisites

Install the latest stable version of Node and Go version 1.20 or higher, and then install the below Go programs. Ensure the GOPATH [bin directory](https://go.dev/doc/gopath_code#GOPATH) is added to your PATH.

Install protobuf
- Check if you've already installed protobuf ```protoc --version```
- If your version is different than v3.21.11, run ```brew unlink protobuf```
- Get v3.21.11 ```curl -s https://raw.githubusercontent.com/Homebrew/homebrew-core/9de8de7a533609ebfded833480c1f7c05a3448cb/Formula/protobuf.rb > /tmp/protobuf.rb```
- Install it ```brew install /tmp/protobuf.rb```
- Check out your version ```protoc --version```

Install protoc-gen-go and protoc-gen-go-rpc:

- Install protoc-gen-go v1.28.1 ```go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28.1```
(Note that this will install a binary in $GOBIN so make sure $GOBIN is in your $PATH)

- Install protoc-gen-go-grpc v1.2.0 ```go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2.0```

```bash
$ make dep
$ make tools
```

### Build

First step is to build the user interface artifacts:

```bash
$ pushd web
$ yarn install
$ yarn build
$ popd
```

After that, you can build the Harness binary:

```bash
$ make build
```

### Run

This project supports all operating systems and architectures supported by Go.  This means you can build and run the system on your machine; docker containers are not required for local development and testing.

To start the server at `localhost:3000`, simply run the following command:

```bash
./gitness server .local.env
```

### Auto-Generate Harness API Client used by UI using Swagger
Please make sure to update the autogenerated client code used by the UI when adding new rest APIs.

To regenerate the code, please execute the following steps:
- Regenerate swagger with latest Harness binary `./gitness swagger > web/src/services/code/swagger.yaml`
- navigate to the `web` folder and run `yarn services`

The latest API changes should now be reflected in `web/src/services/code/index.tsx`

# Run Registry Conformance Tests
```
make conformance-test
```
For running conformance tests with existing running service, use:
```
make hot-conformance-test
```

## User Interface

This project includes a full user interface for interacting with the system. When you run the application, you can access the user interface by navigating to `http://localhost:3000` in your browser.

## REST API

This project includes a swagger specification. When you run the application, you can access the swagger specification by navigating to `http://localhost:3000/swagger` in your browser (for raw yaml see `http://localhost:3000/openapi.yaml`).
For registry endpoints, currently swagger is located on different endpoint `http://localhost:3000/registry/swagger/` (for raw json see `http://localhost:3000/registry/swagger.json`). These will be later moved to the main swagger endpoint. 


For testing, it's simplest to just use the cli to create a token (this requires Harness server to run):
```bash
# LOGIN (user: admin, pw: changeit)
$ ./gitness login

# GENERATE PAT (1 YEAR VALIDITY)
$ ./gitness user pat "my-pat-uid" 2592000
```

The command outputs a valid PAT that has been granted full access as the user.
The token can then be send as part of the `Authorization` header with Postman or curl:

```bash
$ curl http://localhost:3000/api/v1/user \
-H "Authorization: Bearer $TOKEN"
```


## CLI
This project includes VERY basic command line tools for development and running the service. Please remember that you must start the server before you can execute commands.

For a full list of supported operations, please see
```bash
$ ./gitness --help
```

## Contributing

Refer to [CONTRIBUTING.md](https://github.com/harness/harness/blob/main/CONTRIBUTING.md)

## License

Apache License 2.0, see [LICENSE](https://github.com/harness/harness/blob/main/LICENSE).

---

## 📜 License & Copyright

**Copyright © 2026 BlackRoad OS, Inc. All Rights Reserved.**

**CEO:** Alexa Amundson

**PROPRIETARY AND CONFIDENTIAL**

This software is the proprietary property of BlackRoad OS, Inc. and is **NOT for commercial resale**.

### ⚠️ Usage Restrictions:
- ✅ **Permitted:** Testing, evaluation, and educational purposes
- ❌ **Prohibited:** Commercial use, resale, or redistribution without written permission

### 🏢 Enterprise Scale:
Designed to support:
- 30,000 AI Agents
- 30,000 Human Employees
- One Operator: Alexa Amundson (CEO)

### 📧 Contact:
For commercial licensing inquiries:
- **Email:** blackroad.systems@gmail.com
- **Organization:** BlackRoad OS, Inc.

See [LICENSE](LICENSE) for complete terms.

---

<div align="center">

## 🖤 BlackRoad OS Integration

This project is part of the **BlackRoad OS** ecosystem - a comprehensive platform for decentralized infrastructure, AI systems, and next-generation computing.

<p>
  <a href="https://blackroad.io">🌐 blackroad.io</a> •
  <a href="https://github.com/BlackRoad-OS">📦 GitHub</a> •
  <a href="mailto:blackroad.systems@gmail.com">📧 Contact</a>
</p>

**© 2026 BlackRoad OS, Inc. All rights reserved.**

</div>
