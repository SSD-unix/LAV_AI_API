# 🧠 Token-Protected ChatBot API

A simple REST API for interacting with a token-authenticated chatbot powered by Flask, SQLite, and a machine learning model (TF-IDF + Logistic Regression). Trained on hundreds of conversational inputs, this API allows secure, smart chatbot interactions.

---

## 🌐 API Overview

URL:  
`https://apishniki.pythonanywhere.com`

### 🛡 Authentication
All requests require a **valid token**.

Tokens must be pre-registered in the SQLite database. Unauthorized tokens will be rejected with a `403` error.

---

## 🔑 1.client

To authorize users, insert tokens into the database like so:

```python
#api client
import requests

BASE_URL = "https://apishniki.pythonanywhere.com"

def chat(token, question):
    url = f"{BASE_URL}/chat"
    payload = {
        "token": token,
        "question": question
    }
    resp = requests.post(url, json=payload)
    if resp.status_code == 401:
        print("error: not working token.")
        return None
    elif resp.status_code != 200:
        print(f"server; error: {resp.status_code}")
        return None
    return resp.json()

if __name__ == "__main__":
    token = input("write your token: ").strip()
    while True:
        question = input("you: ")
        if question.lower() in ['exit', 'quit']:
            break
        response = chat(token, question)
        if response:
            print("AI:", response['bot']['description'])
```

## 🔑 1.get token 

```
import requests

API_URL = "https://apishniki.pythonanywhere.com"
REGISTER_URL = f"{API_URL}/register"

def register_token():
    res = requests.post(REGISTER_URL)
    if res.status_code == 200:
        token = res.json()["token"]
        print(f"✅ your token is: {token}")
        return token
    else:
        print(f"❌ error: {res.text}")
        return None

def main():
    print("🚀 creating new user...")
    token = register_token()
    if not token:
        return

if __name__ == "__main__":
    main()

```
## 📸 Interface Preview

![Interface](https://github.com/SSD-unix/LAV_AI_API/blob/main/Screenshot%20From%202025-08-20%2005-53-15.png?raw=true)
