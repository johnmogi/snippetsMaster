1. npx create-react-app client --template typescript
cd client
2. npm i react-bootstrap bootstrap react-router-dom @types/react-router-dom
3. folder structure:  
index.tsx (layout)
import 'bootstrap/dist/css/bootstrap.min.css';
components:
layout
header
sidebar
main
footer
4. index.tsx:  
import React from "react";
import ReactDOM from "react-dom";
import "bootstrap/dist/css/bootstrap.min.css";
import { Layout } from "./components/layout/layout";
ReactDOM.render(<Layout />, document.getElementById("root"));
5. layout.tsx:  
import React, { Component } from "react";
import "./layout.css";
export class Layout extends Component {
  render() {
    return (
      <div className="layout container-fluid">
        <h2 className="title">layout</h2>
      </div>
    );  }}
6. insert navbar into header as an added component
7. build extended layout (updated version):  
import React, { Component } from "react";
import "./layout.css";
import { BrowserRouter, NavLink, Switch, Route } from "react-router-dom";
import Navbar from "react-bootstrap/Navbar";
import { Home } from "../home/home";
export class Layout extends Component {
  render() {
    return (
      <div className="layout container-fluid">
        <BrowserRouter>
          <h2 className="title">layout</h2>
          <Navbar bg="dark">
            <NavLink to="/home" exact>
              Home
            </NavLink>
          </Navbar>
          <main className="jumbotron">
            <Switch>
              <Route path="/" component={Home} exact />
            </Switch>
          </main>
        </BrowserRouter>
      </div>    );  }}
8. updated layout :  
[updated layout]:  
import React, { Component } from "react";
import "./layout.css";
import { BrowserRouter, NavLink, Switch, Route } from "react-router-dom";
import { Home } from "../home/home";
import { Reviews } from "../reviews/reviews";
import Navbar from "react-bootstrap/Navbar";
export class Layout extends Component {
  render() {
    return (
      <div className="layout container-fluid">
        <BrowserRouter>
          <Navbar bg="dark" expand="lg" variant="dark">
            <Navbar.Brand href="/">Resto Crit</Navbar.Brand>
            <NavLink to="/" exact>
              Home
            </NavLink>
            <NavLink to="/reviews" exact>
              Reviews
            </NavLink>
          </Navbar>
          <main className="jumbotron">
            <Switch>
              <Route path="/" component={Home} exact />
              <Route path="/reviews" component={Reviews} exact />
            </Switch>
          </main>
        </BrowserRouter>
      </div>    );  }}
9. take notice not to invest in design- only when time permits and all other jobs are done.
10. build model:  
export class RestoModel {
  public constructor(public restName?: string) {}
}
10. display all items:  
reviews (partial):  
import React, { Component } from "react";
import "./reviews.css";
import { ReviewModel } from "../models/review-model";
// import { NavLink } from "react-router-dom";
interface reviewsState {
  reviews: ReviewModel[];
}
export class Reviews extends Component<any, reviewsState> {
  public constructor(props: any) {
    super(props);
    this.state = {
      reviews: []     };   }
  public componentDidMount(): void {
    fetch("http://localhost:3100/api/reviews")
      .then(response => response.json())
      .then(reviews => this.setState({ reviews }))
      .catch(err => alert(err.message));
  }
  public render(): JSX.Element {
    return (
      <div className="reviews">
        <h2>Here are our {this.state.reviews.length} reviews</h2>
        {this.state.reviews.map(rev => (
          // <NavLink to={"/salaries-per-emp/" + rev.id} key={rev.id}>
          <div className="rev">
            date: {rev.date} <br />
            restCode: {rev.restCode} <br />
            review: {rev.review} <br />
            visitor: {rev.visitor} <br />
          </div>
          // </NavLink>
        ))}
      </div>     );   } }

 
11. FAIL insert + select - retrace:  
import React, { Component, ChangeEvent } from "react";
import "./insert.css";
import { EmployeeModel } from "../../models/employee";
import { SalaryModel } from "../../models/salary";
interface InsertState {
    employees: EmployeeModel[],
    salary: SalaryModel
}
export class Insert extends Component<any, InsertState> {
    public constructor(props: any) {
        super(props);
        this.state = {
            employees: [],
            salary: new SalaryModel()
        };
    }
    public componentDidMount(): void {
        fetch("http://localhost:3000/api/employees")
            .then(response => response.json())
            .then(employees => this.setState({ employees }))
            .catch(err => alert(err.message));
    }
    private setEmployeeID = (args: ChangeEvent<HTMLSelectElement>) => {
        const employeeID = +args.target.value;
        const salary = { ...this.state.salary };
        salary.employeeID = employeeID;
        this.setState({ salary });
    };
    private setDate = (args: ChangeEvent<HTMLInputElement>) => {
        const date = args.target.value;
        const salary = { ...this.state.salary };
        salary.date = date;
        this.setState({ salary });
    };
    private setSalary = (args: ChangeEvent<HTMLInputElement>) => {
        const sal = +args.target.value;
        const salary = { ...this.state.salary };
        salary.salary = sal;
        this.setState({ salary });
    };
    private addSalary = () => {
        const options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                "Accept": "application/json"
            },
            body: JSON.stringify(this.state.salary)
        };
        fetch("http://localhost:3000/api/salaries", options)
            .then(response => response.json())
            .then(salary => alert("Salary has been added. ID: " + salary.id))
            .catch(err => alert(err.message));
    };
    public render(): JSX.Element {
        return (
            <div className="insert">
                <h2>Insert new Salary: </h2>
                <form>
                    <select onChange={this.setEmployeeID}>
                        <option disabled selected>Select Employee</option>
                        {this.state.employees.map(emp =>
                            <option key={emp.id} value={emp.id}>
                                {emp.firstName + " " + emp.lastName}
                            </option>
                        )}
                    </select>
                    <br /><br />
                    <input type="date" onChange={this.setDate} value={this.state.salary.date} />
                    <br /><br />
                    <input type="number" onChange={this.setSalary} value={this.state.salary.salary} />
                    <br /><br />
                    <button type="button" onClick={this.addSalary}>Add Salary</button>
                </form>
            </div>         );     } }


// todo 404