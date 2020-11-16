# SampleCode
This sample code comes from “Roomba Coding Challenge”. See https://github.com/trtincher/Roomba-Coding-Challenge for the full project.

This code is a great example of integrating bootstrap into a custom form. What  is unique here is that the form is populated with the data from the appropriately formatted json file. This json file can be uploaded via the ‘File’ component with a file search or ‘drag and drop’ functionality.

```js
import React, { useState, useEffect } from "react";
import "./Form.css";

import Files from "react-files";

import Form from "react-bootstrap/Form";
import Button from "react-bootstrap/Button";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";

function FormComponent({ input, setInput }) {
  const [form, setForm] = useState({
    ...input,
  });
  const [file, setFile] = useState({});

  useEffect(() => {
    if (file.jsonFile !== undefined) {
      let formatedFile = file.jsonFile;
      formatedFile.dirtLocations = formatedFile.dirtLocations.map(
        (location) => `[${location}]`
      );
      formatedFile.dirtLocations = `${formatedFile.dirtLocations}`;
      setForm(formatedFile);
    }
  }, [file]);

  const handleSubmit = (e) => {
    e.preventDefault();
    setInput(form);
  };

  const handleChange = (e) => {
    let name = e.target.name;
    let value = e.target.value;
    if (name !== "dirtLocations") value = value.split(",");

    setForm({
      ...form,
      [name]: value,
    });
  };

  let fileReader = new FileReader();
  fileReader.onload = (event) => {
    setFile({ jsonFile: JSON.parse(event.target.result) });
  };

  return (
    <div className="formContainer">
      <div className="files">
        <Files
          className="files-dropzone"
          onChange={(file) => fileReader.readAsText(file[file.length - 1])}
          onError={(err) => console.log(err)}
          accepts={[".json"]}
          multiple
          maxFiles={20}
          maxFileSize={10000000}
          minFileSize={0}
          clickable
        >
          <p>Drop files here or click to upload</p>
          <i className="fas fa-file-upload"></i>
        </Files>
      </div>

      <Form onSubmit={handleSubmit} className="form">
        <Row>
          <Col>
            <Form.Group controlId="room-dimensions">
              <Form.Label>Room Dimensions</Form.Label>
              <Form.Control
                type="text"
                placeholder="[10, 10]"
                value={form.roomDimensions}
                name="roomDimensions"
                onChange={handleChange}
              />
            </Form.Group>
          </Col>
          <Col>
            <Form.Group controlId="initial-roomba-location">
              <Form.Label>Initial Roomba Location</Form.Label>
              <Form.Control
                type="text"
                placeholder="[1, 1]"
                value={form.initialRoombaLocation}
                name="initialRoombaLocation"
                onChange={handleChange}
              />
            </Form.Group>
          </Col>
        </Row>
        <Form.Group controlId="dirt-locations">
          <Form.Label>Dirt Locations</Form.Label>
          <Form.Control
            type="text"
            placeholder="[1, 2], [3, 5], [5, 5], [7, 9]"
            value={form.dirtLocations}
            name="dirtLocations"
            onChange={handleChange}
          />
        </Form.Group>
        <Form.Group controlId="driving-instructions">
          <Form.Label>Driving Instructions</Form.Label>
          <Form.Control
            type="text"
            placeholder="N, E, E, N, N, S..."
            value={form.drivingInstructions}
            name="drivingInstructions"
            onChange={handleChange}
          />
        </Form.Group>
        <h5>Hit Submit to See Results!</h5>
        <div className="buttonDiv">
          <Button variant="primary" type="submit">
            Submit
          </Button>
        </div>
      </Form>
    </div>
  );
}

export default FormComponent;
```
