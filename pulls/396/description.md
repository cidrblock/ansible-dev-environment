This pull request adds comprehensive support for parsing and handling Ansible collection requests specified as Git URLs, including those with optional dependencies. It introduces new utility functions for Git URL detection and parsing, updates the collection request parsing logic to correctly distinguish between Git URLs and local paths, and provides an extensive test suite to ensure robust handling of various Git URL formats and edge cases.

**Git URL support in collection parsing:**

* Added `is_git_url` function to reliably detect whether a string is a Git URL, supporting multiple formats (e.g., git+https, git+ssh, https, git@) (`src/ansible_dev_environment/collection.py`).
* Added `parse_git_url_collection_name` function to extract the collection namespace and name from a Git URL using pattern matching and heuristics (`src/ansible_dev_environment/collection.py`).

**Collection request parsing enhancements:**

* Updated `parse_collection_request` to handle Git URLs both with and without optional dependencies, ensuring correct assignment of namespace, name, source, and local flags (`src/ansible_dev_environment/collection.py`).

**Environment variable inheritance for Git executable:**

* Added `**os.environ` to the environment dictionary when running `ansible-galaxy` commands in both `_install_galaxy_collections` and `_install_local_collection` methods (`src/ansible_dev_environment/subcommands/installer.py`).
* This ensures that the Git executable can be found in the system PATH when `ansible-galaxy` needs to clone repositories from Git URLs.
* Without inheriting the parent process environment, `ansible-galaxy` would fail to locate the `git` command, preventing installation of collections from Git sources.

**Testing and validation:**

* Added a new test module `tests/unit/test_collection_git_url.py` with parameterized tests for Git URL detection, collection name parsing, and integration with the collection request parser, covering various formats and edge cases.