## Open the Browser and Go to  (In the Browser Where Open5GS Is Installed).
> http://localhost:9999

## Credentials
```bash
Username: admin
Password: 1423
```

## Click on add subscriber
- You could see the window popped up showing the below wordings. Carefully fill them as shown below.

### 1. Add IMSI Number
```bash
001010000000001

15 digit number.
```
- The IMSI (International Mobile Subscriber Identity) is a unique number stored on your SIM card that identifies you as a mobile subscriber in a cellular network (15 digit number).



### 2. Add Subscriber key and Operator Key.

```
Subscriber Key: 6874736969202073796d4b2079650a73
Operator Key: 504f20634f6320504f50206363500a4f 
```
- Add the following for simplicity (We use the same to configure SIM)


### 3. In the apn/dnn section

> Now we are going to use `srsapn` so add same. and IPv4 protocol.

## Now create the user. 
- You can see the user created in main dashboard. 
