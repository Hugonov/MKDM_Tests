# MKDM_Tests
Tests

**Query #1**

    SELECT firstname, lastname, contract_start_date
    FROM users
    WHERE contract_start_date BETWEEN '2017-01-01' AND '2019-12-31'
    ORDER BY contract_start_date DESC;

| firstname | lastname | contract_start_date |
| --------- | -------- | ------------------- |
| Laurence  | Giovani  | 2019-06-09          |
| Larry     | Parker   | 2019-05-07          |
| Luther    | castard  | 2019-01-27          |
| Josianne  | Picketi  | 2018-09-17          |
| Mohammed  | mergaoui | 2018-06-12          |
| Marie     | Feiber   | 2018-03-03          |
| Marie     | Hubert   | 2018-02-28          |
| Thomas    | quartier | 2017-07-21          |
| Simone    | vuilloz  | 2017-03-19          |
| Paul      | Martel   | 2017-01-12          |

**Query #2**

    SELECT lastname, firstname, countries.label, expertises.label
    FROM profiles
    INNER JOIN users ON user_id = users.id
    INNER JOIN countries ON country_code = countries.code
    INNER JOIN expertises ON expertise_code = expertises.code
    ORDER BY countries.label;

| lastname  | firstname | label      | label           |
| --------- | --------- | ---------- | --------------- |
| Planter   | Nicole    | Belgique   | Salarié         |
| quartier  | Thomas    | Espagne    | Salarié         |
| Giovani   | Laurence  | Espagne    | Salarié-cadre-4 |
| Parker    | Larry     | Etats Unis | Salarié         |
| Germain   | Pascal    | France     | Direction       |
| Atlas     | Zineb     | France     | Sous-traitant   |
| castard   | Luther    | France     | Salarié-cadre-4 |
| vuilloz   | Simone    | France     | Salarié-cadre-2 |
| Hubert    | Marie     | France     | Salarié         |
| André     | Maurice   | France     | Salarié         |
| Mauribert | Aimé      | France     | Salarié         |
| Gaston    | François  | France     | Salarié         |
| Picketi   | Josianne  | France     | Salarié         |
| Sandoro   | Michael   | France     | Salarié         |
| Jertoux   | Xavier    | France     | Salarié         |
| Feiber    | Marie     | France     | Salarié         |
| Gonstan   | Dexter    | France     | Salarié         |
| Martel    | Paul      | France     | Salarié         |
| mergaoui  | Mohammed  | Letonie    | Salarié         |

**Query #3**

    SELECT *
    FROM expertises
    WHERE code != 'SCP05';

| code  | label           | coeff | has_rtt | has_bonus |
| ----- | --------------- | ----- | ------- | --------- |
| SCP01 | Stagiaire       | 0.20  | 1       | 0         |
| SCP02 | Apprenti        | 0.50  | 1       | 0         |
| SCP03 | Alternant       | 0.80  | 1       | 0         |
| SCP04 | Sous-traitant   | 1.00  | 0       | 0         |
| SCP06 | Salarié-cadre   | 1.15  | 1       | 1         |
| SCP07 | Salarié-cadre-2 | 1.20  | 1       | 1         |
| SCP08 | Salarié-cadre-3 | 1.30  | 1       | 1         |
| SCP09 | Salarié-cadre-4 | 1.50  | 1       | 1         |
| SCP10 | Direction       | 2.20  | 1       | 1         |

**Query #4**

    SELECT  *
    FROM expertises
    WHERE has_bonus = 0;

| code  | label         | coeff | has_rtt | has_bonus |
| ----- | ------------- | ----- | ------- | --------- |
| SCP01 | Stagiaire     | 0.20  | 1       | 0         |
| SCP02 | Apprenti      | 0.50  | 1       | 0         |
| SCP03 | Alternant     | 0.80  | 1       | 0         |
| SCP04 | Sous-traitant | 1.00  | 0       | 0         |

**Query #5**

    SELECT lastname, firstname, SUM(tasks.id)
    FROM users
    INNER JOIN profiles ON profiles.user_id = users.id
    INNER JOIN tasks ON tasks.profile_id = profiles.id
    GROUP BY lastname, firstname;

| lastname  | firstname | SUM(tasks.id) |
| --------- | --------- | ------------- |
| Martel    | Paul      | 15            |
| Gonstan   | Dexter    | 76            |
| Feiber    | Marie     | 162           |
| Jertoux   | Xavier    | 342           |
| Planter   | Nicole    | 440           |
| Giovani   | Laurence  | 450           |
| quartier  | Thomas    | 660           |
| Sandoro   | Michael   | 1095          |
| Picketi   | Josianne  | 1416          |
| Gaston    | François  | 909           |
| Mauribert | Aimé      | 1338          |
| André     | Maurice   | 1098          |
| castard   | Luther    | 1729          |
| Hubert    | Marie     | 1595          |
| vuilloz   | Simone    | 1716          |
| Germain   | Pascal    | 1837          |
| Parker    | Larry     | 2327          |
| mergaoui  | Mohammed  | 1131          |
| Atlas     | Zineb     | 579           |


**Query #6**

    SELECT  users.firstname, users.lastname, SUM(tasks.completion) SumCompletion, SUM(tasks.id)SumTotal
    FROM profiles
    CROSS JOIN tasks
    CROSS JOIN users
    WHERE tasks.completion = 100
    GROUP BY tasks.profile_id, users.firstname, users.lastname
    ORDER BY SumTotal DESC, SumCompletion DESC
    LIMIT 1;
    
| firstname | lastname | SumCompletion | SumTotal |
| --------- | -------- | ------------- | -------- |
| Michael   | Sandoro  | 9500          | 14877    |
    
**Query #7**

    SELECT DISTINCT users.firstname, users.lastname
    FROM tasks
    INNER JOIN profiles ON profiles.id = tasks.profile_id
    INNER JOIN users ON users.id = profiles.user_id
    WHERE due_date < completion_date
    GROUP BY users.firstname, users.lastname;

| firstname | lastname  |
| --------- | --------- |
| Paul      | Martel    |
| Dexter    | Gonstan   |
| Marie     | Feiber    |
| Xavier    | Jertoux   |
| Nicole    | Planter   |
| Laurence  | Giovani   |
| Thomas    | quartier  |
| Michael   | Sandoro   |
| Josianne  | Picketi   |
| François  | Gaston    |
| Aimé      | Mauribert |
| Maurice   | André     |
| Luther    | castard   |
| Marie     | Hubert    |
| Simone    | vuilloz   |
| Pascal    | Germain   |
| Larry     | Parker    |
| Mohammed  | mergaoui  |
| Zineb     | Atlas     |

**Query #8**

    SELECT DISTINCT users.firstname, users.lastname, AVG(completion_date - due_date) result
    FROM tasks
    INNER JOIN profiles ON profiles.id = tasks.profile_id
    INNER JOIN users ON users.id = profiles.user_id
    GROUP BY users.firstname, users.lastname
    ORDER BY result DESC
    LIMIT 1;

| firstname | lastname | result          |
| --------- | -------- | --------------- |
| Zineb     | Atlas    | 6867333333.3333 |
