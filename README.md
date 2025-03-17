# <img src="./assests/Asset 3.png" alt="drawing" width="30"/> Remediate

## :earth_americas: Overview

The project contains only one component - _Remediate_.\
_Remediate_ is a component that provides integration with the remediation process of [Root.io](http://root.io).\
It serves, as a stage in the CI/CD pipeline, to remediate the vulnerabilities found in the newly built image.

## :tickets: Component inputs

| Name                    | Description                                                                      | Required | Default                                                   |
|------------------------|--------------------------------------------------------------------------------|----------|-----------------------------------------------------------|
| `image_reference`      | The reference to the image that should be scanned                             | ❌ No     | `ghcr.io/${{ github.repository }}:${{ github.ref_name }}` |
| `org_id`               | The organization ID where the image is stored                                 | ✅ Yes    | `-`                                                       |
| `api_token`            | The API token for authenticating with Root.io                                 | ✅ Yes    | `-`                                                       |
| `registry_credentials_id` | The ID of the registry credentials used to pull the image               | ✅ Yes    | `-`                                                       |
| `output_path`          | The path where remediation artifacts will be stored                          | ❌ No     | `${{ github.workspace }}/remediation-output`              |

## :hammer_and_wrench: Prerequisites

<!-- dprint-ignore-start -->
1. Root.io account - go to [app.root.io](https://app.root.io) and create new account.\
   \
   <img src="./assests/create org.gif" alt="drawing" width="680"/>


2. Registry integration - click on integrations in the left menu and create a new registry integration.\
   \
   <img src="./assests/create integration.gif" alt="drawing" width="680"/>

## :checkered_flag: Getting started


1. Go to [root.io integration](https://app.root.io/settings/integrations), press the "Connect to your CI/CD" button and follow the instructions


2. `IMAGE_REFERENCE` - Set the image reference to the image that should be scanned - e.g. `rootio/demo:$CI_COMMIT_TAG`.\
   \
   <img src="./assests/cicd.gif" alt="drawing" width="680"/>
<!-- dprint-ignore-end -->

## :bulb: Example

```yaml
name: Remediation Pipeline

on:
   pull_request:
   push:
      branches:
         - main
      tags:
         - "*"

jobs:
   remediate:
      runs-on: ubuntu-latest
      steps:
         - name: Checkout repository
           uses: actions/checkout@v4

         - name: Run Root.io Remediation
           if: ${{ github.event_name == 'push' || startsWith(github.ref, 'refs/tags/') }}
           uses: rootio-avr/rootio-remediation-action@latest
           with:
              org_id: ${{ env.ROOTIO_ORG_ID }}
              api_token: ${{ secrets.ROOTIO_API_TOKEN }}
              registry_credentials_id: ${{ env.ROOTIO_REGISTRY_CREDENTIALS }}
              image_reference: "ghcr.io/${{ github.repository }}:${{ github.ref_name }}"
              output_path: "${{ github.workspace }}/custom-output-path"
```

## :white_check_mark: Project status

This project is being maintained and is actively being worked on by
[Root.io](http://root.io) :seedling:. If you are using it or have feedback,
we'd love to hear from you.

## :scroll: License

MIT License

Copyright (c) 2025 Root.io

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

<img src="./assests/Asset 2.svg" alt="drawing" width="300"/>
