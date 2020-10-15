# Smartcrypt SDK Docs

## Building
The documentation uses MKDocs, and is built using Python. Run `mkdocs serve -f <mkdocs-language.yml>` to preview the site.

It is highly recommended to use a Python virtual environment when running the script.
```sh
# Create the virtual environment
python3 -m venv path/to/new/virtual/environment

# Activate the virtual environment
source path/to/virtual/environment/bin/activate

# Install requirements if they have changed.
pip install -r requirements.txt

# cd to working directory
mkdocs serve -f mkdocs-java.yml

# If you changed dependencies, save them to requirements.
pip freeze > requirements.txt

# Exit the virtual environment
deactivate
```
We recommend naming the virtual environment `venv` as this has been added to the `.gitignore`.

## Contributing
Javadocs go into `java/api`. Remove the old ones with `git rm -rf <folder>` to make sure that changes get tracked accurately between updates.

## Deploying
Docs are deployed by a CI job. They can be viewed at https://sdk.smartcrypt.com/.
