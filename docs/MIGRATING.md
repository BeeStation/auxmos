# 0.2 to 0.3

If you're using generic fires, `fire_enthalpy_released` was replaced with a more general `enthalpy`. If you're not, you don't need to do anything in auxgm.

# 0.3 to 1.0

New functions were added:

1. `/datum/gas_mixture/proc/adjust_moles_temp(gas_type, amt, temperature)`
2. `/datum/gas_mixture/proc/adjust_multi()` (it's variadic, of the form `gas1, amt1, gas2, amt2, ...`)
3. `/datum/gas_mixture/proc/add(amt)`
4. `/datum/gas_mixture/proc/subtract(amt)`
5. `/datum/gas_mixture/proc/multiply(factor)`
6. `/datum/gas_mixture/proc/divide(factor)`
7. `/datum/gas_mixture/proc/__remove_by_flag(taker, flag, amount)` should be paired with a proper remove_by_flag, like remove and remove_ratio
8. `/datum/gas_mixture/proc/get_by_flag(flag)`

There's also new feature flags:

1. `turf_processing`: on by default. Enables the hooks for turf processing, heat processing etc. Required for katmos, of course.
2. `zas_hooks`: Adds a `/datum/gas_mixture/proc/share_ratio(sharer, ratio, share_size, one_way = FALSE)` hook.

Monstermos is now deprecated. Use katmos instead. It inherently has explosive decompression, sorry.

`fire_products = "plasma_fire"` should be replaced with `fire_products = 0` or, preferably, `fire_products = FIRE_PRODUCT_PLASMA` or similar, with `FIRE_PRODUCT_PLASMA` being `#define FIRE_PRODUCT_PLASMA 0`. String conversion like this is why fires weren't working on linux before; this breaking change is required for it not to be a total hack.

# 1.1 to 2.0

1. `equalization` feature flag renamed to `fastmos`, since that's what everyone calls it; if you used `equalization`, change it to `fastmos`
2. `monstermos`, `putnamos`, `explosive_decompression`, `putnamos_decompression` feature flags have been removed entirely (this is why it's 2.0.0); use `fastmos` or `katmos` instead.
3. `--package auxmos` needs to be added due to the repo restructuring.
4. New function: `/datum/gas_mixture/proc/__auxtools_parse_gas_string`. Call it from `parse_gas_string` with the string as the argument and it'll parse it much faster in Rust, which should reduce load times a bunch.
5. **Adjacencies list has been reworked**. Instead of being a bitfield reference to the direction of the adjacency, it is now flags. The flags should be ATMOS_ADJACENT_ANY = 1 and ATMOS_ADJACENT_FIRELOCK = 2. **Adjacency code must be rewritten with this in mind for auxmos to work.** You're going to have to do some weird stuff with firelock code. If you're not using katmos, you can get away with just replacing instances of `= dir` or similar in ImmediateCalculateAdjacentTurfs with `= 1`. **You will need both flags set for it to work when there's a firelock (1 | )**

# 2.3 - 2.4.0
1. New hook `/datum/controller/subsystem/air/proc/equalize_turfs_auxtools` required for feature `katmos`
2. New hook `/datum/controller/subsystem/air/proc/process_excited_groups_auxtools` required for feature `processing`
3. `--package auxmos` flag is no longer needed
4. Flag ATMOS_ADJACENT_ANY = 1 is no longer needed to be set for the adjacent turf to be added

5. The following functions now only take one argument, `TICK_REMAINING_MS` of the master controller
`/datum/controller/subsystem/air/proc/finish_turf_processing_auxtools()`
`/datum/controller/subsystem/air/proc/equalize_turfs_auxtools()`
`/datum/controller/subsystem/air/proc/process_turfs_auxtools()`
`/datum/controller/subsystem/air/proc/post_process_turfs_auxtools()`
`/datum/controller/subsystem/air/proc/process_turf_equalize_auxtools()`
`/datum/controller/subsystem/air/proc/process_excited_groups_auxtools()`

# 2.4.0 - 2.5.0
1. Generating a `bindings.dm` file is now needed, please generate it with `cargo t generate_binds` and include it in your repo, I personally include it in `__DEFINES`
2. Arguments that each binded function takes is now shown
3. `update_air_ref()` now requires a flag again, `-1` for closed turfs, `1` for planets, `2` for regular turfs
4. You need to call each process functions on the air subsystem for them to actually run
