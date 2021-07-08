![question](https://i.imgur.com/6cKJrmo.png)

1) go to the link

2) ![login](https://i.imgur.com/CR4FOkw.png)

3) default credentials don't work and there isn't anything suspicious about the source. so i tried a [basic inject](https://www.w3schools.com/sql/sql_injection.asp) to see if i can get the login page to return TRUE for me 

4) user: `' or '1' == '1` password: `' or '1' == '1`

5) This gave us the flag!![flag](https://i.imgur.com/6gXh7Fv.png)

Flag: ```bcactf{s0_y0u_f04nd_th3_fl13r?}```