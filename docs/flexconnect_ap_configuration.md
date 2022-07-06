# Configuration for connection of Flexconnect Access Points

## Congiration on ISE

### 1. Configure Authorization Profile
An Authorization Profile "Access Point" to be created (applied after profiling an Access Point on ISE):
**Policy > Policy Elements > Results > Authorization Profiles**:
![image](https://user-images.githubusercontent.com/60174786/177613838-e69e3a11-f58f-4644-9542-50a1e70ba228.png)

### 2. Configure Authentication Policy
Since MAB is used for 2nd authentication (on a switchport), similar to CWA case, the following must be configured in "Authentication Policy":<br>
**if User not found - CONTINUE**
![image](https://user-images.githubusercontent.com/60174786/177613298-9554ea3f-5e05-4724-9975-29f7385a8b21.png)

### 3. Create Authorization Policy for Access Points
Create Authorization Policy to apply Authorization Profile "Access Point":
![image](https://user-images.githubusercontent.com/60174786/177614108-9d81b872-3131-402a-82fe-242d402b6311.png)

### 4. Create Authorization Policy for Wireless clients
Create Authorization Policy to apply Authorization Profile "Permit Access" for wireless clients connecting via switchport (authenticated using MAB):
![image](https://user-images.githubusercontent.com/60174786/177614108-9d81b872-3131-402a-82fe-242d402b6311.png)

## Congiration on switch


## Check RADIUS live logs on ISE
There would be 2 sessions in RADIUS Live logs on ISE for each wireless client:
- 1st authentication - on AP via dot1x / MAB (might be after 2nd authentication)
- 2nd authentiction - on switchport via MAB

![image](https://user-images.githubusercontent.com/60174786/177612944-25a5c3a8-ec9d-4ff9-9a83-9b3be5ef9bac.png)