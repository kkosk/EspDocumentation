Neccessary files and folders found in <link>https://github.com/espressif/esp-matter</link>. Use these functions to implement matter to thermostat project.
This file contains useful functions and files for implementing matter to thermostat listed bellow.
## Callback handles
### File: esp-matter/components/esp_matter/esp_matter_core.h
#### Start matter
```
/** ESP Matter Start
 *
 * Initialize and start the matter thread.
 *
 * @param[in] callback event callback.
 * @param[in] callback_arg private data to pass to callback function, optional argument, by default set to NULL.
 *
 * @return ESP_OK on success.
 * @return error in case of failure.
 */
esp_err_t start(event_callback_t callback, intptr_t callback_arg = static_cast<intptr_t>(NULL));
```

#### Node
```
namespace node {

/** Create raw node
 *
 * @return Node handle on success.
 * @return NULL in case of failure.
 */
node_t *create_raw();

/** Get node
 *
 * @return Node handle on success.
 * @return NULL in case of failure.
 */
node_t *get();

} /* node */
```

#### Endpoint
```
namespace endpoint {

/** Create endpoint
 *
 * This will create a new endpoint with a unique endpoint_id and add the endpoint to the node.
 *
 * @param[in] node Node handle.
 * @param[in] flags Bitmap of `endpoint_flags_t`.
 * @param[in] priv_data (Optional) Private data associated with the endpoint. This will be passed to the
 * attribute_update and identify callbacks. It should stay allocated throughout the lifetime of the device.
 *
 * @return Endpoint handle on success.
 * @return NULL in case of failure.
 */
endpoint_t *create(node_t *node, uint8_t flags, void *priv_data);

/** Get endpoint
 *
 * Get the endpoint present on the node.
 *
 * @param[in] node Node handle.
 * @param[in] endpoint_id Endpoint ID of the endpoint.
 *
 * @return Endpoint handle on success.
 * @return NULL in case of failure.
 */
endpoint_t *get(node_t *node, uint16_t endpoint_id);

/** Get endpoint ID
 *
 * Get the endpoint ID for the endpoint.
 *
 * @param[in] endpoint Endpoint handle.
 *
 * @return Endpoint ID on success.
 * @return Invalid Endpoint ID (0xFFFF) in case of failure.
 */
uint16_t get_id(endpoint_t *endpoint);

} /* endpoint */
```

#### Cluster
```
namespace cluster {
/** Create cluster
 *
 * This will create a new cluster and add it to the endpoint.
 *
 * @param[in] endpoint Endpoint handle.
 * @param[in] cluster_id Cluster ID for the cluster.
 * @param[in] flags Bitmap of `cluster_flags_t`.
 *
 * @return Cluster handle on success.
 * @return NULL in case of failure.
 */
cluster_t *create(endpoint_t *endpoint, uint32_t cluster_id, uint8_t flags);
} /* cluster */
```

#### Attributes
```
namespace attribute {

/** Create attribute
 *
 * This will create a new attribute and add it to the cluster.
 *
 * @param[in] cluster       Cluster handle.
 * @param[in] attribute_id  Attribute ID for the attribute.
 * @param[in] flags         Bitmap of `attribute_flags_t`.
 * @param[in] val           Default type and value of the attribute. Use appropriate elements as per the value type.
 * @param[in] max_val_size  For attributes of type char string and long char string, the size should correspond to the
 *                          maximum size defined in the specification. However, for other types of attributes, this
 *                          parameter remains unused, and therefore the default value is set to 0
 *
 * @return Attribute handle on success.
 * @return NULL in case of failure.
 */
attribute_t *create(cluster_t *cluster, uint32_t attribute_id, uint8_t flags, esp_matter_attr_val_t val,
                    uint16_t max_val_size = 0);

/** Get attribute ID
 *
 * Get the attribute ID for the attribute.
 *
 * @param[in] attribute Attribute handle.
 *
 * @return Attribute ID on success.
 * @return Invalid Attribute ID (0xFFFF'FFFF) in case of failure.
 */
uint32_t get_id(attribute_t *attribute);

/** Set attribute val
 *
 * Set/Update the value of the attribute in the database.
 *
 * @note: Once `esp_matter::start()` is done, `attribute::update()` should be used to update the attribute value.
 *
 * @param[in] attribute Attribute handle.
 * @param[in] val Pointer to `esp_matter_attr_val_t`. Use appropriate elements as per the value type.
 *
 * @return ESP_OK on success.
 * @return error in case of failure.
 */
esp_err_t set_val(attribute_t *attribute, esp_matter_attr_val_t *val);

/** Get attribute val
 *
 * Get the value of the attribute from the database.
 *
 * @param[in] attribute Attribute handle.
 * @param[out] val Pointer to `esp_matter_attr_val_t`. Use appropriate elements as per the value type.
 *
 * @return ESP_OK on success.
 * @return error in case of failure.
 */
esp_err_t get_val(attribute_t *attribute, esp_matter_attr_val_t *val);
} /* attribute */
```


## Attributes
### File: esp-matter/components/esp_matter/esp_matter_attribute.h
#### Thermostat attributes
```
namespace thermostat {
namespace attribute {
attribute_t *create_local_temperature(cluster_t *cluster, nullable<int16_t> value);
attribute_t *create_outdoor_temperature(cluster_t *cluster, nullable<int16_t> value);
attribute_t *create_occupancy(cluster_t *cluster, uint8_t value);
attribute_t *create_abs_min_heat_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_abs_max_heat_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_abs_min_cool_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_abs_max_cool_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_pi_cooling_demand(cluster_t *cluster, uint8_t value);
attribute_t *create_pi_heating_demand(cluster_t *cluster, uint8_t value);
attribute_t *create_hvac_system_type_config(cluster_t *cluster, uint8_t value);
attribute_t *create_local_temperature_calibration(cluster_t *cluster, int8_t value);
attribute_t *create_occupied_cooling_setpoint(cluster_t *cluster, int16_t value);
attribute_t *create_occupied_heating_setpoint(cluster_t *cluster, int16_t value);
attribute_t *create_unoccupied_cooling_setpoint(cluster_t *cluster, int16_t value);
attribute_t *create_unoccupied_heating_setpoint(cluster_t *cluster, int16_t value);
attribute_t *create_min_heat_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_max_heat_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_min_cool_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_max_cool_setpoint_limit(cluster_t *cluster, int16_t value);
attribute_t *create_min_setpoint_dead_band(cluster_t *cluster, int8_t value);
attribute_t *create_remote_sensing(cluster_t *cluster, uint8_t value);
attribute_t *create_control_sequence_of_operation(cluster_t *cluster, uint8_t value, uint8_t min, uint8_t max);
attribute_t *create_system_mode(cluster_t *cluster, uint8_t value, uint8_t min, uint8_t max);
attribute_t *create_thermostat_running_mode(cluster_t *cluster, uint8_t value);
attribute_t *create_start_of_week(cluster_t *cluster, uint8_t value);
attribute_t *create_number_of_weekly_transitions(cluster_t *cluster, uint8_t value);
attribute_t *create_number_of_daily_transitions(cluster_t *cluster, uint8_t value);
attribute_t *create_temperature_setpoint_hold(cluster_t *cluster, uint8_t value);
attribute_t *create_temperature_setpoint_hold_duration(cluster_t *cluster, nullable<uint16_t> value);
attribute_t *create_thermostat_programming_operation_mode(cluster_t *cluster, uint8_t value);
attribute_t *create_thermostat_running_state(cluster_t *cluster, uint16_t value);
attribute_t *create_setpoint_change_source(cluster_t *cluster, uint8_t value);
attribute_t *create_setpoint_change_amount(cluster_t *cluster, nullable<int16_t> value);
attribute_t *create_setpoint_change_source_timestamp(cluster_t *cluster, uint16_t value);
attribute_t *create_occupied_setback(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_occupied_setback_min(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_occupied_setback_max(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_unoccupied_setback(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_unoccupied_setback_min(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_unoccupied_setback_max(cluster_t *cluster, nullable<uint8_t> value);
attribute_t *create_emergency_heat_delta(cluster_t *cluster, uint8_t value);
attribute_t *create_ac_type(cluster_t *cluster, uint8_t value);
attribute_t *create_ac_capacity(cluster_t *cluster, uint16_t value);
attribute_t *create_ac_refrigerant_type(cluster_t *cluster, uint8_t value);
attribute_t *create_ac_compressor_type(cluster_t *cluster, uint8_t value);
attribute_t *create_ac_error_code(cluster_t *cluster, uint32_t value);
attribute_t *create_ac_louver_position(cluster_t *cluster, uint8_t value);
attribute_t *create_ac_coil_temperature(cluster_t *cluster, nullable<int16_t> value);
attribute_t *create_ac_capacity_format(cluster_t *cluster, uint8_t value);
} /* attribute */
} /* thermostat */
```
#### Thermostat UI
```
namespace thermostat_user_interface_configuration {
namespace attribute {
attribute_t *create_temperature_display_mode(cluster_t *cluster, uint8_t value);
attribute_t *create_keypad_lockout(cluster_t *cluster, uint8_t value);
attribute_t *create_schedule_programming_visibility(cluster_t *cluster, uint8_t value);
} /* attribute */
} /* thermostat_user_interface_configuration */
```

## Client
### File: esp-matter/components/esp_matter/esp_matter_client.h

```
namespace thermostat {
namespace command {

using transitions =
    chip::app::DataModel::List<chip::app::Clusters::Thermostat::Structs::WeeklyScheduleTransitionStruct::Type>;

using get_weekly_schedule_callback =
    void (*)(void *, const chip::app::Clusters::Thermostat::Commands::GetWeeklySchedule::Type::ResponseType &);

esp_err_t send_setpoint_raise_lower(peer_device_t *remote_device, uint16_t remote_endpoint_id, uint8_t mode,
                                    uint8_t amount);

esp_err_t send_set_weekly_schedule(peer_device_t *remote_device, uint16_t remote_endpoint_id,
                                   uint8_t num_of_tras_for_seq, uint8_t day_of_week_for_seq, uint8_t mode_for_seq,
                                   transitions &trans);

esp_err_t send_get_weekly_schedule(peer_device_t *remote_device, uint16_t remote_endpoint_id, uint8_t day_to_return,
                                   uint8_t mode_to_return, get_weekly_schedule_callback get_weekly_schedule_cb);

esp_err_t send_clear_weekly_schedule(peer_device_t *remote_device, uint16_t remote_endpoint_id);

} // namespace command
} // namespace thermostat
```

## Cluster
### File: esp-matter/components/esp_matter/cluster.h
#### Thermostat cluster
```
namespace thermostat {
typedef struct config {
    uint16_t cluster_revision;
    nullable<int16_t> local_temperature;
    uint8_t control_sequence_of_operation;
    uint8_t system_mode;
    feature::heating::config_t heating;
    feature::cooling::config_t cooling;
    feature::occupancy::config_t occupancy;
    feature::setback::config_t setback;
    feature::schedule_configuration::config_t schedule_configuration;
    feature::auto_mode::config_t auto_mode;
    feature::local_temperature_not_exposed::config_t local_temperature_not_exposed;
    config() : cluster_revision(6), local_temperature(), control_sequence_of_operation(4), system_mode(1) {}
} config_t;

cluster_t *create(endpoint_t *endpoint, config_t *config, uint8_t flags, uint32_t features);
} /* thermostat */
```
#### Thermostat UI
```
namespace thermostat_user_interface_configuration {
typedef struct config {
    uint16_t cluster_revision;
    uint8_t temperature_display_mode;
    uint8_t keypad_lockout;
    config() : cluster_revision(2), temperature_display_mode(0), keypad_lockout(0) {}
} config_t;

cluster_t *create(endpoint_t *endpoint, config_t *config, uint8_t flags);
} /* thermostat_user_interface_configuration */
```

## Command
### File: esp-matter/components/esp_matter_command.h

```
namespace thermostat {
namespace command {
command_t *create_setpoint_raise_lower(cluster_t *cluster);
command_t *create_set_weekly_schedule(cluster_t *cluster);
command_t *create_get_weekly_schedule(cluster_t *cluster);
command_t *create_clear_weekly_schedule(cluster_t *cluster);
command_t *create_get_weekly_schedule_response(cluster_t *cluster);
} /* command */
} /* thermostat */
```

## Endpoint
### File: esp-matter/components/esp_matter_endpoint.h

```
namespace thermostat {
typedef struct config {
    cluster::descriptor::config_t descriptor;
    cluster::identify::config_t identify;
    cluster::scenes_management::config_t scenes_management;
    cluster::groups::config_t groups;
    cluster::thermostat::config_t thermostat;
} config_t;

uint32_t get_device_type_id();
uint8_t get_device_type_version();
endpoint_t *create(node_t *node, config_t *config, uint8_t flags, void *priv_data);
esp_err_t add(endpoint_t *endpoint, config_t *config);
} /* thermostat */
```

## Features
### File: esp-matter/components/esp_matter_feature.h

```
namespace thermostat {
namespace feature {
```
#### Heating
```
namespace heating {

typedef struct config {
   int16_t occupied_heating_setpoint;

   config (): occupied_heating_setpoint(2000) {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* heating */
```
#### Cooling
```
namespace cooling {

typedef struct config {
   int16_t occupied_cooling_setpoint;

   config (): occupied_cooling_setpoint(2600) {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* cooling */
```
#### Occupancy
```
// Attributes of Occupancy feature may have dependency on Heating, Cooling and Setback
// feature, one must add features according to the usecase first.
namespace occupancy {

typedef struct config {
   uint8_t occupancy;
   int16_t unoccupied_cooling_setpoint;
   int16_t unoccupied_heating_setpoint;
   nullable<uint8_t> unoccupied_setback;
   nullable<uint8_t> unoccupied_setback_min;
   nullable<uint8_t> unoccupied_setback_max;

   config (): occupancy(1), unoccupied_cooling_setpoint(2600), unoccupied_heating_setpoint(2000), unoccupied_setback(), unoccupied_setback_min(), unoccupied_setback_max() {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* occupancy */
```
#### Schedule
```
namespace schedule_configuration {

typedef struct config {
   uint8_t start_of_week;
   uint8_t number_of_weekly_transitions;
   uint8_t number_of_daily_transitions;

   config (): start_of_week(0), number_of_weekly_transitions(0), number_of_daily_transitions(0) {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* schedule_configuration */
```
#### Setback
```
namespace setback {

typedef struct config {
   nullable<uint8_t> occupied_setback;
   nullable<uint8_t> occupied_setback_min;
   nullable<uint8_t> occupied_setback_max;

   config (): occupied_setback(), occupied_setback_min(), occupied_setback_max() {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* setback */

// Auto feature mandates the Heating and Cooling feature, while adding
// Auto feature one must add Heating and Colling features.
```
#### Auto-mode
```
namespace auto_mode {

typedef struct config {
   int8_t min_setpoint_dead_band;

   config (): min_setpoint_dead_band(25) {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* auto_mode */
```
#### Local temperature not exposed
```
namespace local_temperature_not_exposed {

typedef struct config {
    int16_t local_temperature_calibration;

    config (): local_temperature_calibration(0) {}
} config_t;

uint32_t get_id();
esp_err_t add(cluster_t *cluster, config_t *config);
} /* local_temperature_not_exposed */

```
```
} /* feature */
} /* thermostat */
```

## Flags
### File: esp-matter/components/esp_matter.h

```
namespace esp_matter {
```
#### Endpoint flags
```
/** Endpoint flags */
typedef enum endpoint_flags {
    /** No specific flags */
    ENDPOINT_FLAG_NONE = 0x00,
    /** The endpoint can be destroyed using `endpoint::destroy()` */
    ENDPOINT_FLAG_DESTROYABLE = 0x01,
    /** The endpoint is a bridged node */
    ENDPOINT_FLAG_BRIDGE = 0x02,
} endpoint_flags_t;
```
#### Cluster flags
```
/** Cluster flags */
typedef enum cluster_flags {
    /** No specific flags */
    CLUSTER_FLAG_NONE = 0x00,
    /** The cluster has an init function (function_flag) */
    CLUSTER_FLAG_INIT_FUNCTION = CLUSTER_MASK_INIT_FUNCTION, /* 0x01 */
    /** The cluster has an attribute changed function (function_flag) */
    CLUSTER_FLAG_ATTRIBUTE_CHANGED_FUNCTION = CLUSTER_MASK_ATTRIBUTE_CHANGED_FUNCTION, /* 0x02 */
    /** The cluster has a shutdown function (function_flag) */
    CLUSTER_FLAG_SHUTDOWN_FUNCTION = CLUSTER_MASK_SHUTDOWN_FUNCTION, /* 0x10 */
    /** The cluster has a pre attribute changed function (function_flag) */
    CLUSTER_FLAG_PRE_ATTRIBUTE_CHANGED_FUNCTION = CLUSTER_MASK_PRE_ATTRIBUTE_CHANGED_FUNCTION, /* 0x20 */
    /** The cluster is a server cluster */
    CLUSTER_FLAG_SERVER = CLUSTER_MASK_SERVER, /* 0x40 */
    /** The cluster is a client cluster */
    CLUSTER_FLAG_CLIENT = CLUSTER_MASK_CLIENT, /* 0x80 */
} cluster_flags_t;
```
#### Attribute flags
```
/** Attribute flags */
typedef enum attribute_flags {
    /** No specific flags */
    ATTRIBUTE_FLAG_NONE = 0x00,
    /** The attribute is writable and can be directly changed by clients */
    ATTRIBUTE_FLAG_WRITABLE = ATTRIBUTE_MASK_WRITABLE, /* 0x01 */
    /** The attribute is non volatile and its value will be stored in NVS */
    ATTRIBUTE_FLAG_NONVOLATILE = ATTRIBUTE_MASK_NONVOLATILE, /* 0x02 */
    /** The attribute has bounds */
    ATTRIBUTE_FLAG_MIN_MAX = ATTRIBUTE_MASK_MIN_MAX, /* 0x04 */
    ATTRIBUTE_FLAG_MUST_USE_TIMED_WRITE = ATTRIBUTE_MASK_MUST_USE_TIMED_WRITE, /* 0x08 */
    /** The attribute uses external storage for its value. If the ESP Matter data model is used, all the attributes
    have this flag enabled, as all of them are stored in the ESP Matter database. */
    ATTRIBUTE_FLAG_EXTERNAL_STORAGE = ATTRIBUTE_MASK_EXTERNAL_STORAGE, /* 0x10 */
    ATTRIBUTE_FLAG_SINGLETON = ATTRIBUTE_MASK_SINGLETON, /* 0x20 */
    ATTRIBUTE_FLAG_NULLABLE = ATTRIBUTE_MASK_NULLABLE, /* 0x40 */
    /** The attribute read and write are overridden. The attribute value will be fetched from and will be updated using
    the override callback. The value of this attribute is not maintained internally. */
    ATTRIBUTE_FLAG_OVERRIDE = ATTRIBUTE_FLAG_NULLABLE << 1, /* 0x100 */
    /** The attribute is non-volatile but its value will be changed frequently. If an attribute has this flag, its value
     will not be written to flash immediately. A timer will be started and the attribute value will be written after
     timeout. */
    ATTRIBUTE_FLAG_DEFERRED = ATTRIBUTE_FLAG_NULLABLE << 2, /* 0x200 */
} attribute_flags_t;
```
#### Command flags
```
/** Command flags */
typedef enum command_flags {
    /** No specific flags */
    COMMAND_FLAG_NONE = 0x00,
    /** The command is not a standard command */
    COMMAND_FLAG_CUSTOM = 0x01,
    /** The command is client generated */
    COMMAND_FLAG_ACCEPTED = 0x02,
    /** The command is server generated */
    COMMAND_FLAG_GENERATED = 0x04,
} command_flags_t;
```
```
} /* esp_matter */

```