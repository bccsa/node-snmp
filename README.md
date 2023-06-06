# node-snmp
Submodule used to monitor parameters in a nodeJS service with SNMP

## Prerequisites
1. Nodejs
2. net-snmp (npm install net-snmp)
3. dotenv (npm install dotenv --save)

## Environmental variables
1. SNMP_COMUNITY - SNMP comunity to use
2. SNMP_PORT - SNMP port (default: 161)
3. SNMP_TTL - TTL for item added to the SNMP comunity (ms) (used by housekeeper) (default: 90000)
4. SNMP_HOUSE_KEEPER - Interval the SNMP housekeeper shoud run (ms) (default: 1800000)

## MIB id 
1. 1.3.6.1.4.1.54392.5.1464.<entry_id>

## Usage 
```js
const {snmpServer} = require('node-snmp');

const snmp = new snmpServer();

let val_to_monitor = 12;

setInterval(() => { updateData() }, 60000)

function updateData() {
    val_to_monitor ++;
    /**
     * add data to monitor to array of objects
     * type: Value type
     * name: value name 
     * value: value to send
     * maxAccess: type of access snmp has (read-only since module cant write back to nodejs a.t.m.)
     */
    let snmpData = [
        { type: "OctetString", name: "ifName", value: "val_to_monitor", maxAccess: "read-only" },
        { type: "Integer", name: "ifval_to_monitor", value: val_to_monitor, maxAccess: "read-only" }
    ];

    /**
     * Set snmp data to MIB
     * entryID: id used to update the same value at a later stage
     * name: MIB name
     * columns: data added in prev step
     * maxAccess: type of access snmp has (read-only since module cant write back to nodejs a.t.m.)
     */
    snmp.setData({ entryID: data.id, name: "ifval_to_monitor_mib", columns: snmpData, maxAccess: "read-only" });
}
```