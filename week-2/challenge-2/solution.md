# 모범 답안

```python
ace_of_teams = ['JUV : Ronaldo', 'CHE : Hazard', 'LIV : Salah', 'MNU : Pogba', 'INT : Icardi', 'BCN : Suarez', 'RMD : Modric']
podium = []

for player in ace_of_teams:
    player = player[6:].upper()
    podium.append(player)

podium.sort()
print(podium)

```
