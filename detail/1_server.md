1. create server folder
2. cd server
3. create app.js
4. npm init -y
5. npm i cors express joi mysql
6. files and folders:

buisness-logic-layer {rest-logic.js} data-access-layer {dal.js} controllers {rest-controller.js} models {rest.js}

1. create db:

{restotasks}

restaurants: restId, restName reviews: restId, visitor, date, review

add demo data

created db with 2 tables and id connected.

back to server: create the logic:

{app.js} const express = require("express"); const cors = require("cors"); const restController = require("./controllers/rest-controller"); const server = express();

server.use(cors()); server.use(express.json()); server.use("/api", restController);

server.listen(4000, () => console.log("Listening on <http://localhost:4000>"));

connection.connect(err => { if (err) { console.error(err); return; } console.log("We're connected to restotasks on MySQL."); });

// SQL הוצאה לפועל של שאילתת function executeAsync(sql) { return new Promise((resolve, reject) => { connection.query(sql, (err, result) => { if (err) { reject(err); return; } resolve(result); }); }); }

module.exports = { executeAsync };

{controller/rest-controller.js} const express = require("express"); const restsLogic = require("../business-logic-layer/rest-logic"); const router = express.Router();

// GET <http://localhost:3000/api/products> router.get("/restos", async (request, response) => { try { const rests = await restsLogic.getAllRestsAsync(); response.json(rests); } catch (err) { response.status(500).send(err.message); } });

module.exports = router;

{buissness-logic-layer/ rest-logic.js} const dal = require("../data-access-layer/dal");

async function getAllRestsAsync() { const sql = "SELECT * FROM `restaurants`"; const restos = await dal.executeAsync(sql); return restos; } module.exports = { getAllRestsAsync }
