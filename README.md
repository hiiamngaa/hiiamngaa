import React from "react";
import React, { useState, useEffect } from "react";
import axiosInstance from "../../../service";

interface InfoData {
  label: string;
  value: string;
}

interface StarShipData {
  name: string;
  max_atmosphering_speed: string;
  cost_in_credits: string;
  passengers: string;
  films: string[];
}

interface CardProps {
  type: string;
@@ -7,29 +21,70 @@ interface CardProps {
}

const Card = ({ type, onClick, clickable }: CardProps) => {
  const [starShipData, setStarShipData] = useState<StarShipData | null>(null);
  const [otherCardData, setOtherCardData] = useState<StarShipData | null>(null);

  useEffect(() => {
    getRandomStarShip();
  }, []);

  useEffect(() => {
    if (starShipData && otherCardData) {
      // Both card data have been loaded, you can proceed here
      console.log("Both card data loaded:", starShipData, otherCardData);
    }
  }, [starShipData, otherCardData]);

  const getRandomStarShip = async () => {
    try {
      let starShipResponse = null;
      let randomStarShipId = null;

      while (!starShipResponse) {
        randomStarShipId = Math.floor(Math.random() * 40) + 1;
        starShipResponse = await axiosInstance.get(`/${randomStarShipId}`);
      }

      setStarShipData(starShipResponse.data);
    } catch (error) {
      console.log("Error fetching random starship:", error);
    }
  };

  const handleCardClick = () => {
    if (clickable && onClick) {
      onClick();
    }
  };

  const infoData = [
    "Maximum speed",
    "Cost in credits",
    "Number of passengers",
    "Films",
  const infoData: InfoData[] = [
    {
      label: "Max Speed",
      value: starShipData?.max_atmosphering_speed || "",
    },
    {
      label: "Credit Cost",
      value: starShipData?.cost_in_credits || "",
    },
    { label: "Passenger", value: starShipData?.passengers || "" },
    {
      label: "Film Appearances",
      value: starShipData?.films.length.toString() || "",
    },
  ];

  return (
    <div className={`my-card my-card-${type}`} onClick={handleCardClick}>
      <div className={`my-info my-info-${type}-name`}>Name</div>
      <div className={`my-info my-info-${type}-name`}>
        {starShipData?.name || "Loading..."}
      </div>
      <div className={`my-info my-info-img my-info-img-${type}`}>
        <img src="" alt="" />
        <img src="" alt="Image of Starship" />
      </div>
      {infoData.map((info, index) => (
        <div key={index} className={`my-info my-info-${type}`}>
          {info}
          <span>?</span>
          {info.label}
          <span>{info.value}</span>
        </div>
      ))}
    </div>
 255 changes: 129 additions & 126 deletions255  
src/screens/Home/index.css
@@ -1,190 +1,193 @@
@import url('https://fonts.cdnfonts.com/css/retro-groove');
@import url("https://fonts.cdnfonts.com/css/retro-groove");

* {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
}

.my-home-container {
    position: relative;
    height: 100vh;
    width: 100%;
    background-color: rgba(0, 0, 67, 0.499);
    background: url("https://wallpaperaccess.com/full/2724260.jpg");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    font-family: 'Retro Groove', sans-serif;


  position: relative;
  height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 67, 0.499);
  background: url("https://wallpaperaccess.com/full/2724260.jpg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  font-family: "Retro Groove", sans-serif;
}

.my-logo-class {
    position: absolute;
    top: 8%;
    left: 50%;
    transform: translate(-50%, -50%);
    margin-top: 5%;
  position: absolute;
  top: 8%;
  left: 50%;
  transform: translate(-50%, -50%);
  margin-top: 5%;
}

.my-logo {
    width: 150px;
    margin-bottom: 30px;
  width: 150px;
  margin-bottom: 30px;
}

.my-info-score {
    display: flex;
    justify-content: space-around;
    padding: 10px;
  display: flex;
  justify-content: space-around;
  padding: 10px;
}

.my-user-score>h4 {
    font-size: 20px;
    color: blue;
.my-user-score > h4 {
  font-size: 20px;
  color: blue;
}

.my-computer-score>h4 {
    font-size: 20px;
    color: red;
.my-computer-score > h4 {
  font-size: 20px;
  color: red;
}

.my-user {
    padding: 10px;
    width: 150px;
    border: 5px solid blue;
    color: black;
    text-align: center;
    border-radius: 10px;
    background-color: yellow;
    margin-top: 5%;
  padding: 10px;
  width: 150px;
  border: 5px solid blue;
  color: black;
  text-align: center;
  border-radius: 10px;
  background-color: yellow;
  margin-bottom: 5%;
}

.my-computer {
    padding: 10px;
    width: 150px;
    border: 5px solid red;
    color: black;
    text-align: center;
    border-radius: 10px;
    background-color: yellow;
    margin-top: 5%;
  padding: 10px;
  width: 150px;
  border: 5px solid red;
  color: black;
  text-align: center;
  border-radius: 10px;
  background-color: yellow;
  margin-bottom: 5%;
}

.my-round {
    text-align: center;
    margin-top: 50px;
    padding: 10px;
    color: white;
    border-radius: 10px;
    width: 200px;
    margin-left: auto;
    margin-right: auto;
    border: 5px solid yellow;
    background-image: linear-gradient(to right, #0015ff, #ff0000);
    background-origin: border-box;
    background-clip: padding-box, border-box;
  text-align: center;
  padding: 10px;
  color: white;
  border-radius: 10px;
  width: 200px;
  margin-left: auto;
  margin-right: auto;
  border: 5px solid yellow;
  background-image: linear-gradient(to right, #0015ff, #ff0000);
  background-origin: border-box;
  background-clip: padding-box, border-box;
}

.my-round-title>h3 {
    font-size: 20px;
.my-round-title > h3 {
  font-size: 20px;
}

.my-round-count>h4 {
    font-size: 15px;
    color: yellow;
.my-round-count > h4 {
  font-size: 15px;
  color: yellow;
}

.my-computer-name>h3,
.my-user-name>h3 {
    font-size: 20px;
.my-computer-name > h3,
.my-user-name > h3 {
  font-size: 20px;
}

.my-card-container {
    display: flex;
    justify-content: center;
    gap: 100px;
  display: flex;
  justify-content: center;
  gap: 100px;
}

.my-card {
    margin-top: 100px;
    padding: 10px;
    color: black;
    border-radius: 10px;
    width: 300px;
    border: 5px solid yellow;
  margin-top: 100px;
  padding: 10px;
  color: black;
  border-radius: 10px;
  width: 300px;
  border: 5px solid yellow;
}

.my-info {
    /* border: 1px solid white; */
    background-color: white;
    border-radius: 10px;
    margin: 15px;
    padding: 5px;
  /* border: 1px solid white; */
  background-color: white;
  border-radius: 10px;
  margin: 15px;
  padding: 5px;
}

.my-info-user {
    background: linear-gradient(to left, rgb(255, 255, 255) 50%, rgb(229, 255, 0) 50%) right;
    background-size: 200%;
    transition: .1s ease-out;
  background: linear-gradient(
      to left,
      rgb(255, 255, 255) 50%,
      rgb(229, 255, 0) 50%
    )
    right;
  background-size: 200%;
  transition: 0.1s ease-out;
}

.my-info-user:hover {
    /* background-color: rgba(255, 255, 0, 0.881); */
    background-position: left;
    cursor: pointer;
  /* background-color: rgba(255, 255, 0, 0.881); */
  background-position: left;
  cursor: pointer;
}

.my-info-user-name,
.my-info-computer-name {
    text-align: center;
  text-align: center;
}


.my-info-img {
    height: 100px;
  height: 100px;
}

span {
    float: right;
  float: right;
}

.my-card-user {
    background-color: rgba(0, 0, 255, 0.800);
  background-color: rgba(0, 0, 255, 0.8);
}

.my-card-computer {
    background-color: rgba(255, 0, 0, 0.800);
}

@media screen and (max-width: 800px) {
    .my-home-container {
        height: 100%;
    }

    .my-info-score {
        flex-direction: column;
        align-items: center;
    }

    .my-user,
    .my-computer {
        width: 100%;
        margin-bottom: 20px;
    }

    .my-card-container {
        flex-direction: column;
        align-items: center;
    }

    .my-card {
        align-items: center;
        width: 110%;
    }

    .my-logo-class {
        display: none;
    }
}
  background-color: rgba(255, 0, 0, 0.8);
}

@media screen and (max-width: 650px) {
  .my-home-container {
    height: 100%;
  }

  .my-info-score {
    flex-direction: column;
    align-items: center;
  }

  .my-user,
  .my-computer {
    width: 100%;
    margin-bottom: 20px;
  }

  .my-card-container {
    flex-direction: column;
    align-items: center;
    gap: 10px;
  }

  .my-card {
    align-items: center;
    width: 90%;
    margin: 20px;
  }

  .my-logo-class {
    display: none;
  }
}
  12 changes: 7 additions & 5 deletions12  
src/screens/Home/index.tsx
@@ -1,20 +1,22 @@
import React, { useState } from "react";
import { useLocation } from "react-router-dom";
import "./index.css";
import Card from "./Card";
import Round from "./Round";


const Home = () => {

  const [isMusicPlaying, setIsMusicPlaying] = useState(false);
  const location = useLocation();
  const username = location.state?.username || "User";

  const handleCardUserClick = () => {
    console.log("Clicked!");

  };

  const handlePlayMusic = () => {
    const audioElement = document.getElementById('background-music') as HTMLAudioElement;
    const audioElement = document.getElementById(
      "background-music"
    ) as HTMLAudioElement;
    console.log(audioElement);
    audioElement.play();
    setIsMusicPlaying(true);
@@ -36,7 +38,7 @@ const Home = () => {
      <div className="my-info-score">
        <div className="my-user">
          <div className="my-name my-user-name">
            <h3>User</h3>
            <h3>{username}</h3>
          </div>
          <div className="my-score my-user-score">
            <h4>100</h4>
 125 changes: 65 additions & 60 deletions125  
src/screens/Welcome/index.tsx
@@ -1,69 +1,74 @@
import React, { useState } from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';
import './index.css';
import Button from 'react-bootstrap/Button';
import 'bootstrap/dist/css/bootstrap.min.css';
import { useNavigate } from 'react-router-dom';
import React, { useState } from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";
import "./index.css";
import Button from "react-bootstrap/Button";
import "bootstrap/dist/css/bootstrap.min.css";
import { useNavigate } from "react-router-dom";
interface FormData {
    username: string;
    gender: string;
  username: string;
  gender: string;
}

const UserForm = () => {
    const navigate = useNavigate();
    const initialValues: FormData = {
        username: '',
        gender: '',
    };
  const navigate = useNavigate();
  const initialValues: FormData = {
    username: "",
    gender: "",
  };

    const validationSchema = Yup.object().shape({
        username: Yup.string().required('Username is required!'),
        gender: Yup.string().required('Gender is required!'),
    });
  const validationSchema = Yup.object().shape({
    username: Yup.string().required("Username is required!"),
    gender: Yup.string().required("Gender is required!"),
  });

    const handleSubmit = (values: FormData) => {
        console.log('Form data submitted:', values);
        navigate('/game');
    };
  const handleSubmit = (values: FormData) => {
    console.log("Form data submitted:", values);
    navigate("/game", { state: { username: values.username } });
  };

    return (
        <Formik
            initialValues={initialValues}
            validationSchema={validationSchema}
            onSubmit={handleSubmit}

        >
            <Form className='form'>
                <h1 className='header'>Welcome to The Star Wars</h1>
                <div className='my-user-container'>
                    <div className="form-field">

                        {/* Sử dụng Field thay cho input */}
                        <Field className="name" type="text" id="name" name="username" placeholder="Enter your name" required />
                        {/* Sử dụng ErrorMessage thay cho div */}
                        <ErrorMessage name="username" component="div" className="error" />

                    </div>
                    <div className="form-field">
                        <label >
                            <Field className='my-user-select' as="select" name="gender">
                                <option value="">Select gender</option>
                                <option value="male">Male</option>
                                <option value="female">Female</option>
                                <option value="other">Other</option>
                            </Field>
                            <ErrorMessage name="gender" component="div" className="error" />
                        </label>
                    </div>
                    <div className="form-field">
                        <Button type='submit' variant="danger">Play</Button>{' '}
                    </div>
                </div>

            </Form>
        </Formik>
    );
  return (
    <Formik
      initialValues={initialValues}
      validationSchema={validationSchema}
      onSubmit={handleSubmit}
    >
      <Form className="form">
        <h1 className="header">Welcome to The Star Wars</h1>
        <div className="my-user-container">
          <div className="form-field">
            {/* Sử dụng Field thay cho input */}
            <Field
              className="name"
              type="text"
              id="name"
              name="username"
              placeholder="Enter your name"
              required
            />
            {/* Sử dụng ErrorMessage thay cho div */}
            <ErrorMessage name="username" component="div" className="error" />
          </div>
          <div className="form-field">
            <label>
              <Field className="my-user-select" as="select" name="gender">
                <option value="">Select gender</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Other</option>
              </Field>
              <ErrorMessage name="gender" component="div" className="error" />
            </label>
          </div>
          <div className="form-field">
            <Button type="submit" variant="danger">
              Play
            </Button>{" "}
          </div>
        </div>
      </Form>
    </Formik>
  );
};

export default UserForm;
export default UserForm;
 2 changes: 1 addition & 1 deletion2  
src/service/index.ts
@@ -1,6 +1,6 @@
import axios from 'axios';

const axiosInstance = axios.create({
    baseURL: 'https://swapi.dev/api/people',
    baseURL: 'https://swapi.dev/api/starships/',
  });
  export default axiosInstance;
