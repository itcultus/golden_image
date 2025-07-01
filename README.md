# itcultus.golden_images Ansible Collection
![Ansible Galaxy](https://img.shields.io/badge/Ansible_Galaxy-itcultus.golden_images-red?logo=ansible)

## Description

This collection provides Ansible content for building and managing RHEL
"golden images" across various virtualization platforms. It aims to standardize 
and automate your image creation pipelines and workflows, ensuring an always 
updated RHEL Golden Image created on your standards.

## Table of Contents

- [Description](#description)
- [Contents](#contents)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Author Information](#author-information)

## Contents

This collection currently includes:
**`image_builder` role::** 
This role is designed to help you build golden images using Red Hat's Image Builder.

## Requirements

To use this collection, you will need:

* Ansible `core >= 2.15`


## Installation

To install the `itcultus.golden_images` collection from Ansible Galaxy:

```bash
ansible-galaxy collection install itcultus.golden_images
```

## Contributing

Contributions are highly welcome! If you find a bug, have a feature request, 
or wish to contribute code, please follow these steps:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/your-feature-name` or `bugfix/your-bug-fix`).
3.  Make your changes.
4.  Write comprehensive tests.
5.  Ensure your code adheres to our coding standards.
6.  Submit a Pull Request (PR) to the `main` branch.

Please see our [CONTRIBUTING.md](CONTRIBUTING.md) for more detailed guidelines.

## License

This collection is licensed under the **GNU General Public License v3.0**. See the [LICENSE](LICENSE) file for the full text.

## Author Information

This collection is maintained by **[Peter Tselios/ITCultus LTD]**
