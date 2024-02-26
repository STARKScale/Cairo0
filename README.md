## To set up the environment for Cairo0
```
python3.9 -m venv ~/cairo_venv
source ~/cairo_venv/bin/activate
```
## To compile mapper code
```
cairo-compile mapper.cairo --output mapper_compiled.json
```

## To compile reducer code
```
cairo-compile reducer.cairo --output reducer_compiled.json
```
## To run the file
```
cairo-run --program=mapper_compiled.json \
    --print_output --layout=small \
```

## To incorporate with the stone prover
```
cairo-run \
    --program=mapper_compiled.json \
    --layout=small \
    --air_public_input=mapper_public_input.json \
    --air_private_input=mapper_private_input.json \
    --trace_file=mapper_trace.json \
    --memory_file=mapper_memory.json \
    --print_output \
    --proof_mode
```

```
cpu_air_prover \
    --out_file=mapper_proof.json \
    --private_input_file=mapper_private_input.json \
    --public_input_file=mapper_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json
```

## Configuration for other input sizes
The number of steps affects the size of the trace. Such changes may require modification of cpu_air_params.json. Specifically, the following equation must be satisfied.

log₂(last_layer_degree_bound) + ∑fri_step_list = log₂(#steps) + 4
For instance, assuming a fixed last_layer_degree_bound, a larger number of steps requires changes to the fri_step_list to maintain the equality.

FRI steps should typically be in the range 2-4; the degree bound should be in the range 4-7.

The constant 4 that appears in the equation is hardcoded log₂(trace_rows_per_step) = log₂(16) = 4