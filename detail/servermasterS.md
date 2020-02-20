{DB:utf8_gen_ci} 
[Linux bug: prevent by making all fields lowercase]
1. utf8 general bin
2. id _ primary ai / re
3. review id as index id index
 / restId as primary + null as default + null checkbox
 [take notice, second primary not AI not Null]
4. attach id on designer 5.add data manually

{SERVER}
1. mkdir server + cd server
2. app.js
3. npm init -y
4. npm i express mysql cors joi
5. create folder logic:

1. dal (data access layer) express db access
2. buisness logic- resto logic ( get all, get one, CRUD)
3. controllers - resto controller ( get. post, routing)
4. app. js - (exspress, connection, server use)
5. DAL - make sql connection with promise and export:<br>
const mysql = require("mysql");
// MySQL יצירת הגדרות התחברות למסד הנתונים
const connection = mysql.createConnection({
    host: "localhost", // מחשב
    user: "root", // היוזר של מסד הנתונים
    password: "q1w2e3", // הסיסמה של מסד הנתונים
    database: "restotasks" // שם מסד הנתונים
});
connection.connect(err => {
    if (err) {
        console.error(err);
        return;
    }
    console.log("We're connected to restotasks on MySQL.");
});
// SQL הוצאה לפועל של שאילתת
function executeAsync(sql) {
    return new Promise((resolve, reject) => {
        connection.query(sql, (err, result) => {
            if (err) {
                reject(err);
                return;
            }
            resolve(result);
        });
    });
}
module.exports = {
    executeAsync
};
6. BLL - "export" the sql query as in crud:  
const dal = require("../data-access-layer/dal");
async function getAllRestsAsync() {
    const sql = "SELECT * FROM `restaurants`";
    const restos = await dal.executeAsync(sql);
    return restos;
}
module.exports = {
    getAllRestsAsync
}
7. CTRL - use the router so that the sql query will be submitted on a url:  
const express = require("express");
const restsLogic = require("../business-logic-layer/rest-logic");
const router = express.Router();
// GET http://localhost:3000/api/products
router.get("/restos", async (request, response) => {
    try {
        const rests = await restsLogic.getAllRestsAsync();
        response.json(rests);
    } catch (err) {
        response.status(500).send(err.message);
    }
});
module.exports = router;
8. app.js connect the cors + listen to the server with express and the controller:  
const express = require("express");
const cors = require("cors");
const restController = require("./controllers/resto-controller");
const server = express();
server.use(cors());
server.use(express.json());
server.use("/api", restController);
server.listen(3100, () => console.log("Listening on http://localhost:3100"));
9. make sure all folders and applications are spelled correctly  
10. test the first api call woth postman- plan ahead- are there additional routes?
11. DB extended notes:  
[if there's a secondary primary id on the 2nd table - it won't accept null- 
allways send an added value]
join tables simplified: 
SELECT * FROM `reviews` JOIN restaurants ON `restId`= id
join tables extended:
SELECT S.id, S.employeeID, S.date, S.salary, E.firstName, E.lastName
        FROM Salaries as S JOIN Employees as E
        ON E.id = S.employeeID
`restCode``restId``date``visitor``review`
        select r.restCode, r.restId, r.date, r.visitor, r.review, s.restname
        from reviews as r join restaurants as s
        on r.id = s.id
12. add an item with ID:  
async function addSalaryAsync(salary) {
    const sql = `INSERT INTO Salaries(employeeID, Date, Salary)
        VALUES(${salary.employeeID},'${salary.date}',${salary.salary})`;
    const info = await dal.executeAsync(sql);
    salary.id = info.insertId;
    return salary;
}
13. final output: insert...  
async function addReviewAsync(review) {
    const sql = `INSERT INTO reviews(restCode,date, visitor, review)
                VALUES(${review.restCode},'${review.date}',${review.visitor},${review.review})`;
    const info = await dal.executeAsync(sql);
    review.id = info.insertId;
    return review;
}
14.  MAKE sure strings are marked with single quote- or it won't post:  
async function addReviewAsync(review) {
    const sql = `INSERT INTO reviews(restCode,date, visitor, review) VALUES(${review.restCode},'${review.date}','${review.visitor}','${review.review}')`;
    const info = await dal.executeAsync(sql);
    review.id = info.insertId;
    return review;
}
