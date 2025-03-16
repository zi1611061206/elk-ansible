Init Python virtual env
```bash
python -m venv venv

# usage
source venv/bin/activate
```

Install reno & setup template in .reno
```bash
pip install reno

# usage
reno new --from-template .reno/template.yml 000001-setup-project
```

Install ansible & log requirements file
```bash
pip install ansible
pip freeze > requirements.txt

# usage
pip install -r requirements.txt
```
