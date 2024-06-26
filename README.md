# gpu-carbon-estimator

`gpu-carbon-estimator` estimates the carbon emissions from the energy usage of a GPU, made as a plugin for [IF](https://github.com/Green-Software-Foundation/if).

## Implementation

The plugin uses the [Climatiq](https://www.climatiq.io/) API to retrieve carbon emission estimations.

## Climatiq API Key

`gpu-carbon-estimator` requires a Climatiq API key, which you can request [here](https://www.climatiq.io/docs/guides/how-tos/getting-api-key). Once you have a key, you must define an environment variable, `$CLIMATIQ_API_KEY=<your-api-key-here>` on your machine.

## Inputs

- `gpu/power-usage`: GPU power usage in watts.
- `region`: Geographical region where the measurement was recorded. Should be a two-character string according to the [UN/LOCODE code list](https://unece.org/trade/cefact/unlocode-code-list-country-and-territory), i.e., `'US'` for the United States, `'GB'` for Great Britain.

## Returns

- `gpu/carbon`: Estimated GPU carbon emissions in kgCO2.

<!-- ## Usage

To run the `gpu-carbon-estimator` plugin an instance of `GpuCarbonEstimator` must be created using `GpuCarbonEstimator()` and, if applicable, passing global configurations. Subsequently, the `execute()` function can be invoked to retrieve data on `gpu/carbon`.

This is how you could run the plugin in Typescript:

```typescript
import {GpuCarbonEstimator} from '@dukeofjukes/gpu-carbon-estimator`

const gpuCarbonEstimator = GpuCarbonEstimator({});
const response = await gpuCarbonEstimator.execute([
  {
    timestamp: '2024-01-01T00:00:00Z',
    duration: 60,
    region: 'GB',
    'gpu/power-usage': 50,
  },
  {
    timestamp: '2024-01-01T00:01:00Z',
    duration: 60,
    region: 'GB',
    'gpu/power-usage': 80,
  },
]);
``` -->

## Example manifest

First, ensure IF is installed:

```sh
npm i -g @grnsft/if
```

Then, clone and link this plugin's repository:

```sh
git clone git@github.com:dukeofjukes/gpu-carbon-estimator.git && cd gpu-carbon-estimator
```

```sh
npm update && npm install && npm link
```

The following manifest initializes and runs the locally-linked `gpu-carbon-estimator` plugin:

```yaml
name: gpu-carbon-estimator-demo
description: estimates carbon emissions from gpu power usage
tags:
initialize:
  plugins:
    gpu-carbon-estimator:
      method: GpuCarbonEstimator
      path: gpu-carbon-estimator
tree:
  children:
    child:
      pipeline:
        - gpu-carbon-estimator
      defaults:
        region: "GB"
      inputs:
        - timestamp: '2024-01-01T00:00:00Z'
          duration: 60  # seconds
          gpu/power-usage: 50  # watts
        - timestamp: '2024-01-01T00:01:00Z'
          duration: 60  # seconds
          gpu/power-usage: 80  # watts
```

You can run this by passing it to `ie`:


```sh
ie --manifest ./path/to/input.yml --output ./path/to/output.yml
```