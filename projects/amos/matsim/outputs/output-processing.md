# MATSim output processing

Scripts to process MATSim events and network files are available in [this](https://github.com/MetacitySuite/Metacity-MATSimOutput) repository.
The output is movement information in SIM format that can by visualized in Metacity and MATSim network converted to ShapeFile.

## SIM files
The .sim format is our dedicated file format which stores movement information and metadata about dynamic subjects. 

Each file contains a list of dynamic subjects. Continouous movement for each subject is stored as a MultiTimePoint - moovement is defined in time by "start" in seconds, and for each next second are stored the subject coordinates in "geometry". The list of coordinates is encoded as base64 string.


### Example:
```
    [
        {   "meta": {
                "start": 32093, 
                "subject_id": "18", 
                "passengers": [9], 
                "veh_type": "bike", 
                "vehicle_id": "veh_1_bike"
                },
            "geometry": "AAAAAAAA+P8AAAAAAAD4/wAAAAAAAPj/AAAAAAAA+P/QNEcvooEmwW2Eiq1W4i"
    `   },
        {   "meta": {
                "start": 35642, 
                "subject_id": "264", 
                "passengers": [245,12], 
                "veh_type": "car", 
                "vehicle_id": "veh_78_car"
                },
            "geometry": "AAAAAAAA+P8AAAAAAAD4/wAAAAAAAPj/AAAAAAAA+P/BnT8bYYOEJsGIzViCIOMvwbJN+DeLhCbBJ3JxmiDjL8HHW9UOk4QmwccWirIg4y/B3Gmy5ZqEJsFmu6LKIOMvwUgPe+mihCbBoCzBfyDjL8G0tEPtqoQmwdud3zQg4y/BIFoM8bKEJsEWD/7pH+MvwYz/1PS6hCbBUIAcnx/jL8H5pJ34woQmwYrxOlQf4y/BZUpm/MqEJsHFYlkJH+MvwdHvLgDThCbBANR3vh7jL8E9lfcD24QmwTpFlnMe4y/BqTrAB+OEJsF0trQoHuMvwRXgiAvrhCbBryfT3R3jL8GBhVEP84QmweqY8ZId4y/B7ioaE/uEJsEkChBIHeMvwVrQ4hYDhSbBXnsu/RzjL8HGdasaC4UmwZnsTLIc4y/BMht0HhOFJsHUXWtnHOMvwZ7APCIbhSbBDs+JHBzjL8Hvom9tI4Umwb9uXLEb4y/BQIWiuCuFJsFwDi9GG+MvwZFn1QM0hSbBIq4B2xrjL8HiSQhPPIUmwdNN1G8a4y/BMyw7mkSFJsGE7aYEGuMvwYQObuVMhSbBNY15mRnjL8HV8KAwVYUmwecsTC4Z4y/BJtPTe12FJsGYzB7CbBMYOIVSLhL8Fh6I7Oc4wmwTkEwL8g4S/Bas5uLnmMJsFBhfcpH+EvwXK0To5+jCbBSgYvlB3hL8F7mi7ug4wmwVKHZv4b4S/Bg4AOTomMJsFaCJ5oGuEvwRsFDTSRjCbBgGtRFBjhL8G"
    `   },
        ...
```

