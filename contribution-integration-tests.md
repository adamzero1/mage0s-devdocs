# Contribution: Integration Tests

[TOC]
- [Introduction](#introduction)
- [Process Explaination](#process-explaination)
  - [Matrix Calulator](#matrix-calulator)
  - [Project Setup](#project-setup)
  - [NX Project Generation](#nx-project-generation)
  - [Cache Generated Project](#cache-generated-project)
  - [Restore Generated Project](#restore-generated-project)
  - [Warden Setup Environment](#warden-setup-environment)
  - [Warden Run Memory Tests](#warden-run-memory-tests)
  - [Warden Run "Fake" Integration Tests](#warden-run-fake-integration-tests)
  - [NX Run "Real" Integration Tests](#nx-run-real-integration-tests)


## Introduction
Mage-OS leverages [Nx](https://nx.dev/) and [Warden](https://docs.warden.dev/) to run integration tests for every pull request. 

## Process Explaination
[![](https://mermaid.ink/img/pako:eNqtVl1vmzAU_SuWH6dGU-03VOWl-9AeMlXppE0aE3LhJnELNjIma1Tlv-9iQoAQPpLVT_jec47tA9zrNxrqCKhHMyssfJJibUQy2zJfERy_P_whs9mcpCYIDSAgKuMOTFbavATl491dMZnPy3QNd-waV6YbvCKdCGvkaxCKOMxjYbU5i8rA5mmQGv0MoT3sohlyIPUarEGBEVZqVYJaIQcKRbiBEyW3zLOW6nieYlKdp7NDp1PDS1RL9yyisUCRNpCh1pFyOwXEpoC4r5rn8uk3ZWF9sOAHYjOfEpERWYcDW4RvyVtJLMbp9txKf4WJAJd21oPaSqNVAqrafDF6IU0Bk6sggUSb3WHlDr-DOKWvxAsE3SOcFerBVh9NAcEPNh6UG8Y5KfxfSvz-Ov_ZgP9s0H827j8b8p-N-s-m-c8u8J-N-88m-s_exX8-4D8f9J-P-8-H_Oej_vNp_vML_Ofj_vOJ_vOz_iuN9hu53liiV2RIfAlp2TCw7BevJhFqR0KdPEnlCFkNRaUMzFaGkBG7AbJw1Znci7jZPooRgQWTSFW1LVCR21JRHuse5ZGHJbmv2lun1ntnFmh1Ho88FlPyUHWUVsvxyPdfB8BHVErJ10aDarUMD1fAaQVAL46Kp6XYQ79cZAKYXQLmg-Deyu6Rny51OOjnOtVPYteQ-ARSp3EcOctckYXLHKpAH4VdTuHTKD2NqEX26RdEYYnqlq1xKfZ-Unyy1HBrdL9AKbFExBUS7P8l-CQJekMTLBlCRngldr3Ap1hjEjy3h48RrEQeW5_6ao9QkVv9uFMh9azJ4YbmaVRfoqm3EnGGUYgk_lCL8prtbtv7f4_vIGw?type=png)](https://mermaid.live/edit#pako:eNqtVl1vmzAU_SuWH6dGU-03VOWl-9AeMlXppE0aE3LhJnELNjIma1Tlv-9iQoAQPpLVT_jec47tA9zrNxrqCKhHMyssfJJibUQy2zJfERy_P_whs9mcpCYIDSAgKuMOTFbavATl491dMZnPy3QNd-waV6YbvCKdCGvkaxCKOMxjYbU5i8rA5mmQGv0MoT3sohlyIPUarEGBEVZqVYJaIQcKRbiBEyW3zLOW6nieYlKdp7NDp1PDS1RL9yyisUCRNpCh1pFyOwXEpoC4r5rn8uk3ZWF9sOAHYjOfEpERWYcDW4RvyVtJLMbp9txKf4WJAJd21oPaSqNVAqrafDF6IU0Bk6sggUSb3WHlDr-DOKWvxAsE3SOcFerBVh9NAcEPNh6UG8Y5KfxfSvz-Ov_ZgP9s0H827j8b8p-N-s-m-c8u8J-N-88m-s_exX8-4D8f9J-P-8-H_Oej_vNp_vML_Ofj_vOJ_vOz_iuN9hu53liiV2RIfAlp2TCw7BevJhFqR0KdPEnlCFkNRaUMzFaGkBG7AbJw1Znci7jZPooRgQWTSFW1LVCR21JRHuse5ZGHJbmv2lun1ntnFmh1Ho88FlPyUHWUVsvxyPdfB8BHVErJ10aDarUMD1fAaQVAL46Kp6XYQ79cZAKYXQLmg-Deyu6Rny51OOjnOtVPYteQ-ARSp3EcOctckYXLHKpAH4VdTuHTKD2NqEX26RdEYYnqlq1xKfZ-Unyy1HBrdL9AKbFExBUS7P8l-CQJekMTLBlCRngldr3Ap1hjEjy3h48RrEQeW5_6ao9QkVv9uFMh9azJ4YbmaVRfoqm3EnGGUYgk_lCL8prtbtv7f4_vIGw)

https://mermaid.live/edit#pako:eNqtVl1vmzAU_SuWH6dGU-03VOWl-9AeMlXppE0aE3LhJnELNjIma1Tlv-9iQoAQPpLVT_jec47tA9zrNxrqCKhHMyssfJJibUQy2zJfERy_P_whs9mcpCYIDSAgKuMOTFbavATl491dMZnPy3QNd-waV6YbvCKdCGvkaxCKOMxjYbU5i8rA5mmQGv0MoT3sohlyIPUarEGBEVZqVYJaIQcKRbiBEyW3zLOW6nieYlKdp7NDp1PDS1RL9yyisUCRNpCh1pFyOwXEpoC4r5rn8uk3ZWF9sOAHYjOfEpERWYcDW4RvyVtJLMbp9txKf4WJAJd21oPaSqNVAqrafDF6IU0Bk6sggUSb3WHlDr-DOKWvxAsE3SOcFerBVh9NAcEPNh6UG8Y5KfxfSvz-Ov_ZgP9s0H827j8b8p-N-s-m-c8u8J-N-88m-s_exX8-4D8f9J-P-8-H_Oej_vNp_vML_Ofj_vOJ_vOz_iuN9hu53liiV2RIfAlp2TCw7BevJhFqR0KdPEnlCFkNRaUMzFaGkBG7AbJw1Znci7jZPooRgQWTSFW1LVCR21JRHuse5ZGHJbmv2lun1ntnFmh1Ho88FlPyUHWUVsvxyPdfB8BHVErJ10aDarUMD1fAaQVAL46Kp6XYQ79cZAKYXQLmg-Deyu6Rny51OOjnOtVPYteQ-ARSp3EcOctckYXLHKpAH4VdTuHTKD2NqEX26RdEYYnqlq1xKfZ-Unyy1HBrdL9AKbFExBUS7P8l-CQJekMTLBlCRngldr3Ap1hjEjy3h48RrEQeW5_6ao9QkVv9uFMh9azJ4YbmaVRfoqm3EnGGUYgk_lCL8prtbtv7f4_vIGw

### Matrix Calulator
https://github.com/adamzero1/mageos-magento2/blob/2.4-develop/.github/workflows/integration-tests.yml#L28-L44
This processes the [supported-services.json][https://github.com/adamzero1/mageos-magento2/blob/2.4-develop/supported-services.json], to work out all possible combinations of services that Mage-OS supports.
For example, if we supported:
- PHP 7.4 or PHP 8.1
- Redis 5.0.4 or Redis 6.0.0

Then we would get the following
- PHP7.4 + Redis 5.0.4
- PHP7.4 + Redis 6.0.0
- PHP8.1 + Redis 5.0.4
- PHP8.1 + Redis 6.0.0

As you can imagine this generates a lot of combinations, far more then anyone would want to test manually.

### Project Setup
Here the project is setup (from the HEAD of the PR), `git clone`, `composer install` is ran etc...

### NX Project Generation
NX is added to the project, then used to generate a map.

### Cache Generated Project
The output of the previous two steps is cached.

### Restore Generated Project
The cache output is restored, this way the code base is ready to go with no further steps requied.

### Warden Setup Environment
Warden docker environment is setup: 

### Warden Run Memory Tests
As there isn't a way to run the memory tests for specific modules.
https://github.com/adamzero1/mageos-magento2/blob/2.4-develop/dev/tests/integration/phpunit.xml.dist#L35-L37

### Warden Run "Fake" Integration Tests
As there isn't a way to run for specific modules
https://github.com/adamzero1/mageos-magento2/blob/2.4-develop/dev/tests/integration/phpunit.xml.dist#L31-L33

### NX Run "Real" Integration Tests

https://github.com/adamzero1/mageos-magento2/blob/2.4-develop/dev/tests/integration/phpunit.xml.dist#L38-L43

## How is this different to Magento
Crucially the underlying tests being ran are the same, however by leveraging NX to only run tests for modules that have changed the time taken is substantially less (Running a full suite can take >10 hours, whereas only running the tests for modules that have changed may only be a few minutes)
Because we have the time saving we are able to run the tests across every iteration of the supported services on PR, something Magento is unable to do at the moment.


## Current Challenges / Issues
- [x] ~The integration tests live in multiple locations `app/code/*/*/Test/Integration` and `testsuite` (so we can't just run PHPUnit on a specific directory)~
  This is currently being handled by specifying a filter like: `--filter='/Magento/AdminAnalytics/|Magento\\AdminAnalytics'`
- [ ] ~Getting the error "No Alive Nodes Found" when trying to run the integration tests~
  This was because elasticsearch wasn't ready 

## TODO
- correct all github links
