
code
```java
package decrypt_AES;

  

import java.math.BigInteger;

  

public class Decrypt_RSA {

  

public static void main(String[] args) {

// TODO Auto-generated method stub

BigInteger n = new BigInteger("3174654383");

BigInteger e = new BigInteger("65537");

BigInteger C = new BigInteger("2487688703");

BigInteger p = null;

BigInteger q = null;

BigInteger phi = null; //phi=(p-1)(q-1)

BigInteger d = null;

BigInteger M = null;

for(BigInteger i = new BigInteger("2"); i.compareTo(n)<0; i=i.add(BigInteger.ONE)) {

if(n.mod(i).equals(new BigInteger("0"))) {

p=i;

q=n.divide(p);

phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));

System.out.println("p: "+p+", q:"+ q + ", phi:" + phi);

break;

}

}

//ed = 1 mod (p-1)(q-1) = 1 mod phi

d=e.modPow(new BigInteger("-1"), phi);

System.out.println("d = " + d);

M = C.modPow(d, n); //M = c^d mod N

System.out.println("M :" + M);

System.out.println(M.toString(16));

String hexString = M.toString(16);

byte[] bytes = new byte[hexString.length() / 2];

for (int i = 0; i < hexString.length(); i += 2) {

bytes[i / 2] = (byte) Integer.parseInt(hexString.substring(i, i + 2), 16);

}

String message = new String(bytes);

System.out.println("decrypt Message: " + message);

}

  

}
```
실행 결과
![[Pasted image 20250408212536.png]]
Plaintext: 1198485348 -> 변환하면 Good
d: 801567233