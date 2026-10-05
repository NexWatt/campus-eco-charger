# Campus Eco-Charger ☀️🔋

An intelligent, safe, low-voltage hybrid solar tracking and battery storage utility charging station designed for sustainable mobile device deployment.

## 📐 System Architecture Diagram

```mermaid
flowchart TD
    %% Define System Groups/Layers for Visual Clarity
    subgraph INPUTS ["🔌 Energy Generation Inputs"]
        solar["☀️ Solar Panel Input<br>(Fluctuating Voltage)"]
        wall["🔌 Grid Wall Adapter<br>(Steady 5V Backup Power)"]
    end

    subgraph TELEMETRY ["📋 Analog Sensing & Power Routing Layers"]
        divider["📋 Resistor Divider<br>(Scales Voltage to Safe 0-3.3V)"]
        shunt["📊 Shunt Current Circuit<br>(Measures Current Draw)"]
        power_switch{{"⚡ MOSFET Array<br>(Smart Power-Path Gate)"}}
    end

    subgraph BRAIN ["💻 Central Control Core"]
        main_mcu["💻 TNU PIC18 MCU<br>(Primary Control Brain)"]
    end

    subgraph DISPLAY ["📊 Serial Telemetry Output Network"]
        i2c_bus["〰️ Shared I2C Serial Data Bus 〰️"]
        sec_mcu["🤖 Secondary MCU<br>(Screen Driver)"]
        display[("📺 Status Telemetry<br>LCD Screen")]
    end

    %% Electrical Power & Signal Flow Routing
    solar --> divider
    solar --> shunt
    wall --> power_switch
    
    divider -->|"Safe Scaled Voltage"| main_mcu
    shunt -->|"Safe Current Signals"| main_mcu
    
    main_mcu <-->|"Controls Gate Switching"| power_switch
    main_mcu -->|"Pushes Power Data"| i2c_bus
    
    i2c_bus --> sec_mcu
    sec_mcu --> display

    %% Professional Color Styling Schemes
    classDef inputStyle fill:#fff0f5,stroke:#ff69b4,stroke-width:2px;
    classDef senseStyle fill:#f0f8ff,stroke:#1e90ff,stroke-width:2px;
    classDef brainStyle fill:#f0fff0,stroke:#32cd32,stroke-width:3px;
    classDef displayStyle fill:#f5f5dc,stroke:#8b8682,stroke-width:2px;

    class solar,wall inputStyle;
    class divider,shunt,power_switch senseStyle;
    class main_mcu brainStyle;
    class i2c_bus,sec_mcu,display displayStyle;
```

## 🎯 Key Project Features
*   **Safe Low-Voltage Conversion:** Optimizes unstable solar panel inputs down to a stable 5V output channel via a highly efficient Buck converter stage.
*   **Dual-Source Smart Power Routing:** Features an integrated solid-state MOSFET switching array to automatically hot-swap power paths between a local battery storage bank and a grid wall-adapter backup line without interrupting device delivery.
*   **Under-Voltage Lockout (UVLO):** Executes a hardware-driven finite state machine (FSM) to isolate and disconnect depleting battery cells before they drop past critical thresholds and suffer permanent cell degradation.
*   **Dual-MCU Communications Bus:** Utilizes a shared I2C data bus running between a primary TNU PIC18 control microcontroller and a secondary screen-driver MCU to feed live electrical metrics to a local telemetry display screen.
