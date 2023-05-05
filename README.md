```
import SwitchText from "./SwitchText";
import React, { useState, useEffect } from "react";
import * as hi from "react-icons/hi";
import Switch from "react-switch";
import Axios from "axios";
import HoverComponent from "./HoverComponent/HoverComponent";

function SwitchContainer({
  profile,
  setQuickSelect,
  switchStates,
  setSwitchStates,
  toggleStates,
}) {
  const [checked, setChecked] = useState(false);
  // const profile=useParams();
  // const [switchStates, setSwitchStates] = useState([]);
  // const fetchreview = async () => {
  //   const { data } = await Axios.get(
  //     `http://localhost:2610/getUser/getUser/${profile}`
  //   );
  //   setSwitchStates(data);
  //   console.log(data);
  // };

  // useEffect(() => {
  //   fetchreview();
  // }, []);
  const handleToggle = (index, value) => {
    const newswitchStates = [...switchStates];
    newswitchStates[index].contactForm = value;
    setSwitchStates(newswitchStates);
    saveToggleState(index, value); // Save the toggle state to MongoDB
  };
  const saveToggleState = (index, value) => {
    fetch(`http://localhost:2610/getUser/toggle/${switchStates[index]._id}`, {
      method: "PUT",
      body: JSON.stringify({ contactForm: value }),
      headers: {
        "Content-Type": "application/json",
      },
    });
  };
  const handleQuickToggle = (index, value) => {
    const newswitchStates = [...switchStates];
    newswitchStates[index].quickSelect = value;
    setSwitchStates(newswitchStates);
    setQuickSelect(value);
    saveQuickToggleState(index, value); // Save the toggle state to MongoDB
    if (value) {
      Axios.put(`http://localhost:2610/getUser/quickselect`, {
        profile: profile,
      })
        .then((response) => console.log(response))
        .catch((error) => console.log(error));
    }
  };
  const saveQuickToggleState = (index, value) => {
    fetch(
      `http://localhost:2610/getUser/quicktoggle/${switchStates[index]._id}`,
      {
        method: "PUT",
        body: JSON.stringify({ quickSelect: value }),
        headers: {
          "Content-Type": "application/json",
        },
      }
    );
  };

  // hover state 1
  const [isHovering, setIsHovering] = useState(false);
  const handleMouseOver = () => {
    setIsHovering(true);
    console.log("true");
  };

  const handleMouseOut = () => {
    setIsHovering(false);
    console.log("false");
  };
  // hover state 2
  const [isHovering2, setIsHovering2] = useState(false);
  const handleMouseOver2 = () => {
    setIsHovering2(true);
    console.log("true");
  };

  const handleMouseOut2 = () => {
    setIsHovering2(false);
    console.log("false");
  };

  return (
    <div
      className="flex flex-col xl:flex-row justify-between gap-3 px-4 py-3 my-3 mx-2 xl:mx-6 relative"
      style={{ background: "#FAFAFA", borderRadius: " 8px" }}
    >
      <div className="w-full flex items-center gap-3">
        <p className=" font-light s-color ">Allow Lead Capture</p>
        <div className="flex items-center gap-2">
          <div
            className="s-color "
            onMouseOver={handleMouseOver}
            onMouseLeave={handleMouseOut}
            style={{ cursor: "pointer" }}
          >
            <hi.HiOutlineInformationCircle />
            {isHovering && <HoverComponent />}
          </div>
          {switchStates.map((off, index) => (
            <>
              <Switch
                checked={off.contactForm}
                onChange={(value) => handleToggle(index, value)}
                onColor="#12A26E"
                offColor="#A7A7A7"
                checkedIcon={false}
                uncheckedIcon={false}
                height={24}
                width={44}
              />
            </>
          ))}
        </div>
      </div>

      <div class="relative bg-slate-300 w-[2px] mx-2 hidden xl:block" />

      <div className="w-full flex xl:justify-end items-center gap-3">
        <p className=" font-light s-color ">Quick Select</p>
        <div className="flex items-center gap-2">
          <div
            onMouseOver={handleMouseOver2}
            onMouseLeave={handleMouseOut2}
            className="s-color"
            style={{ cursor: "pointer" }}
          >
            <hi.HiOutlineInformationCircle />
            {isHovering2 && (
              <HoverComponent
                label="Quick Select"
                text="You can display all of the links below on your Tapop profile by enabling 'Quick select'."
              />
            )}
          </div>
            {switchStates.map((off, index) => (
              <>
                {toggleStates.length > 0 && (
                  <Switch
                    checked={off.quickSelect}
                    onChange={(value) => handleQuickToggle(index, value)}
                    onColor="#12A26E"
                    offColor="#A7A7A7"
                    checkedIcon={false}
                    uncheckedIcon={false}
                    height={24}
                    width={44}
                  />
                )}
                {toggleStates.length === 0 && (
                  <Switch
                    checked={false}
                    onChange={() => { }}
                    onColor="#12A26E"
                    offColor="#A7A7A7"
                    checkedIcon={false}
                    uncheckedIcon={false}
                    height={24}
                    width={44}
                    disabled
                  />
                )}
              </>
            ))}
        </div >
      </div>
    </div>
  );
}

export default SwitchContainer;

```
