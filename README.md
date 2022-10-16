# NFT Aggregator
NFT's in Solana


**NFT aggregator** -  is a program that provides any information to solana nft by entering the address of the token. The information is sent to the database using an API request from moralis.io and then the information is displayed on the site. The program itself runs in Python. Connecting to a Postgres database (Pg admin4). I also use html, css, bootstrap for the site.

**Moralis.io** - provides world-class APIs to developers across the globe, allowing companies and projects of all sizes to seamlessly integrate blockchain into their solutions stack and scale with ease. [Moralis.io](https://moralis.io/) is here.




## Installation

**Python:**
Install the current version of Python: [PyCharm](https://www.jetbrains.com/ru-ru/pycharm/)


Version: 2022.2.3
Assembly: 222.4345.23
October 11, 2022


**Postgres:** [Pg Admin 4](https://www.pgadmin.org/download/)


Version 6.1 (4280.88)


***Terminal in PyCharm:*** 
```bash
pip install psycopg2-binary
```
```bash
pip install flask
```
```bash
pip install requests
```
```bash
pip install flask-request
```

***In PgAdmin4***: 

Open the PostgreSQL 14 and ***CREATE NEW DATABASE and CALL IT "homedb""***.
Open the ***homedb database*** >>> ***Schemas*** >>> ***Tables*** >> right mouse button and click ***Query Tool*** >>> ***Query Editor*** >>> ***Paste it***:

```
CREATE TABLE nft (address VARCHAR(950), info VARCHAR(950))
```

## Usage
***Solana art:***
You can get the address of the token for the nft aggregator by going to the site and choosing any nft token.
[Solana.art](https://solanart.io/)

***

To get contract level metadata (minta, standard, name, symbol, metaplex) for a given network and contract, use this snippet responsible for this:
```python
import requests

url = "https://solana-gateway.moralis.io/nft/network/address/metadata"

headers = {"accept": "application/json"}

response = requests.get(url, headers=headers)

print(response.text)
```



## Example
***

***My codes with different conditions:***



The first code was made with flask python and a snippet from moralis.io. It works without a database:
```python
from flask import Flask
import requests
from flask import render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def form_example():
    if request.method == 'POST':
        nftaddress = request.form.get('nftaddress')

        url = f'https://solana-gateway.moralis.io/nft/mainnet/' + nftaddress + '/metadata'
        headers = {"accept": "application/json",
                   "X-API-Key": "u8emWI08OHGRqpKRzmO3Y3gW4OhbTdOdVRuJobooGeSvYRGjep6bmjuIDVu8RqEI"}

        response = requests.get(url, headers=headers)

        return f''' 
         <h1>infp: {response.text} </h1>
                         '''
    return '''
              <form method="POST">
                  <div><label>Get address: <input type="text" name="nftaddress"></label></div>
                  <input type="submit" value="Submit">
              </form>'''


if __name__ == '__main__':
    app.run(debug=True)
```


The second code was made with a database but without a snippet from moralis.io:

```python
import psycopg2
from flask import Flask
from flask import render_template, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def form_example():
    if request.method == 'POST':


       connection = psycopg2.connect(user="postgres",
                              password="3008",
                              host="127.0.0.1",
                              port="5432",
                              database="homedb")
       cursor = connection.cursor()
       address = request.form.get('nftaddress')
       postgreSQL_select_Query = "SELECT info FROM nft WHERE address=" + "'" + address + "'" + ";"
       cursor.execute(postgreSQL_select_Query)
       mobile_records = cursor.fetchall()

    for row in mobile_records:

       return f''' 
              <h1>infp: {row[0]} </h1>
       
                             '''

       cursor.close()
       connection.close()


    return '''
                      <form method="POST">
                          <div><label>Get address: <input type="text" name="nftaddress"></label></div>
                          <input type="submit" value="Submit">
                      </form>'''

if __name__ == '__main__':
 app.run(debug=True)
```

***
***Examples for use:***

Run main.py.

Click localhost in terminal, and and the program will open your browser.

You need paste it token(from solana art) in "nft address" and click "Get info".

After clicking on the site, information about your nft that you entered earlier will be displayed.








