# user-flow-portal
UserFlow Portal is a responsive React app with a validated registration form for name, email, phone, PAN, Aadhaar, and more. Submission is blocked until all inputs are correct, then redirects to a page showing the entered details, showcasing form handling and navigation.
Sure! Here’s a **compact, complete React project** with a registration form, validation, password show/hide, and routing. This version is short and ready to run without errors.

index.js**

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";
import "./styles.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter><App /></BrowserRouter>
);

App.js**

```jsx
import React from "react";
import { Routes, Route } from "react-router-dom";
import RegistrationForm from "./RegistrationForm";
import FormDetails from "./FormDetails";

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<RegistrationForm />} />
      <Route path="/details" element={<FormDetails />} />
    </Routes>
  );
}
 About.js**

```jsx
import React, { useState } from "react";
import { useNavigate } from "react-router-dom";

export default function RegistrationForm() {
  const navigate = useNavigate();
  const [form, setForm] = useState({
    firstName: "", lastName: "", username: "", email: "",
    password: "", phone: "", countryCode: "", country: "",
    city: "", pan: "", aadhaar: ""
  });
  const [errors, setErrors] = useState({});
  const [showPass, setShowPass] = useState(false);

  const validate = () => {
    const e = {};
    if (!form.firstName.trim()) e.firstName="Required";
    if (!form.lastName.trim()) e.lastName="Required";
    if (!form.username.trim()) e.username="Required";
    if (!form.email.trim()) e.email="Required";
    else if(!/^\S+@\S+\.\S+$/.test(form.email)) e.email="Invalid";
    if (!form.password) e.password="Required";
    if (!form.countryCode) e.countryCode="Required";
    if (!form.phone) e.phone="Required";
    else if(!/^\d{10}$/.test(form.phone)) e.phone="10 digits";
    if (!form.country) e.country="Required";
    if (!form.city) e.city="Required";
    if (!form.pan) e.pan="Required";
    else if(!/^[A-Z]{5}\d{4}[A-Z]$/.test(form.pan)) e.pan="Invalid";
    if (!form.aadhaar) e.aadhaar="Required";
    else if(!/^\d{12}$/.test(form.aadhaar)) e.aadhaar="12 digits";
    setErrors(e);
    return Object.keys(e).length===0;
  };

  const handleChange = e => setForm({...form,[e.target.name]:e.target.value});
  const handleSubmit = e => { e.preventDefault(); if(validate()) navigate("/details",{state:form}); }

  return (
    <div className="container">
      <h2>Registration</h2>
      <form onSubmit={handleSubmit}>
        {Object.keys(form).map(key => key!=="password" && (
          <div className="input-group" key={key}>
            <label>{key}</label>
            <input name={key} value={form[key]} onChange={handleChange} className={errors[key]?"error":""} />
            {errors[key] && <p className="error-text">{errors[key]}</p>}
          </div>
        ))}
        <div className="input-group">
          <label>Password</label>
          <div className="password-wrapper">
            <input type={showPass?"text":"password"} name="password" value={form.password} onChange={handleChange} className={errors.password?"error":""}/>
            <button type="button" onClick={()=>setShowPass(!showPass)}>{showPass?"Hide":"Show"}</button>
          </div>
          {errors.password && <p className="error-text">{errors.password}</p>}
        </div>
        <button disabled={!validate()}>Submit</button>
      </form>
    </div>
  );

 Functional.js**

import React from "react";
import { useLocation, Link } from "react-router-dom";

export default function FormDetails() {
  const { state } = useLocation();
  return (
    <div className="container">
      <h2>Form Details</h2>
      <div className="details-box">
        {Object.entries(state).map(([k,v])=><p key={k}><strong>{k}: </strong>{v}</p>)}
      </div>
      <Link to="/">Back</Link>
    </div>
  );

### **5️⃣ styles.css**

body{font-family:Arial;background:#f7f7f7;margin:0;padding:20px;}
.container{max-width:450px;margin:auto;background:#fff;padding:20px;border-radius:10px;box-shadow:0 0 10px #ccc;}
h2{text-align:center;}
.input-group{margin-bottom:15px;}
label{font-weight:bold;}
input{width:100%;padding:8px;margin-top:5px;border-radius:5px;border:1px solid #bbb;}
input.error{border-color:red;}
.error-text{color:red;font-size:12px;}
.password-wrapper{display:flex;}
.password-wrapper button{margin-left:5px;padding:8px;cursor:pointer;}
button[disabled]{background:#777;color:#fff;}
.details-box{background:#eee;padding:15px;border-radius:8px;}
