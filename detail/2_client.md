1. create client folder
2. cd client
3. create-react-app client --typescript
4. npm install react-bootstrap bootstrap

```
create-react-app client --typescript
cd client https://www.npmjs.com/package/react-bulma-components
npm i react-bulma-components
```

npm i react-router-dom @types/react-router-dom

```
מחיקת הקבצים המיותרים: app.css, app.tsx,app.test.tsx, logo.svg, react-app-env.d.ts, serviceworker.ts, setupTests.ts
```

npm i -g json-server מתקין שרת ע"מ לבצע בדיקות מקומיות

json-server products.json --port 3015

nav>.navbar-brand>a.navbar-item>img 

<!-- npm install @material-ui/core npm install typeface-roboto --save npm install @material-ui/icons -->

 npm i @types/react-router-dom

interface FooterState { currentYear: number; }

export class Footer extends Component

<any, footerstate=""> {</any,>

```
public constructor(props: any) {
    super(props);
    this.state = {
        currentYear: new Date().getFullYear()
    };
}

public render(): JSX.Element {
    return (
        <div className="footer">
            <h6>All Rights Reserved © {this.state.currentYear}</h6>
        </div>
    );
}
```

}
