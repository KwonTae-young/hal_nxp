#
# Copyright (c) 2018, NXP
#
# SPDX-License-Identifier: Apache-2.0
#

zephyr_include_directories(.)

zephyr_compile_definitions_ifdef(
  CONFIG_PTP_CLOCK_MCUX
  ENET_ENHANCEDBUFFERDESCRIPTOR_MODE
)

zephyr_library_compile_definitions_ifdef(
  CONFIG_HAS_MCUX_CACHE FSL_SDK_ENABLE_DRIVER_CACHE_CONTROL
)
zephyr_library_sources(fsl_common.c)
zephyr_library_sources(fsl_common_arm.c)

zephyr_library_sources_ifdef(CONFIG_ADC_MCUX_LPADC	fsl_lpadc.c)
zephyr_library_sources_ifdef(CONFIG_CAN_MCUX_FLEXCAN	fsl_flexcan.c)
zephyr_library_sources_ifdef(CONFIG_COUNTER_MCUX_GPT	fsl_gpt.c)
zephyr_library_sources_ifdef(CONFIG_COUNTER_MCUX_PIT	fsl_pit.c)
zephyr_library_sources_ifdef(CONFIG_DISPLAY_MCUX_ELCDIF	fsl_elcdif.c)
zephyr_library_sources_ifdef(CONFIG_DMA_MCUX_EDMA	fsl_edma.c)
zephyr_library_sources_ifdef(CONFIG_DMA_MCUX_EDMA	fsl_dmamux.c)
zephyr_library_sources_ifdef(CONFIG_ENTROPY_MCUX_TRNG	fsl_trng.c)
zephyr_library_sources_ifdef(CONFIG_ETH_MCUX		fsl_enet.c)
zephyr_library_sources_ifdef(CONFIG_GPIO_MCUX_IGPIO	fsl_gpio.c)
if(NOT CONFIG_SOC_MIMXRT1176_CM4)
zephyr_library_sources_ifdef(CONFIG_HAS_MCUX_CACHE	fsl_cache.c)
endif()
zephyr_library_sources_ifdef(CONFIG_I2C_MCUX_LPI2C	fsl_lpi2c.c)
zephyr_library_sources_ifdef(CONFIG_I2S_MCUX_SAI	fsl_sai.c)
zephyr_library_sources_ifdef(CONFIG_I2S_MCUX_SAI	fsl_sai_edma.c)
zephyr_library_sources_ifdef(CONFIG_MEMC_MCUX_FLEXSPI	fsl_flexspi.c)
zephyr_library_sources_ifdef(CONFIG_PWM_MCUX		fsl_pwm.c)
zephyr_library_sources_ifdef(CONFIG_SPI_MCUX_LPSPI	fsl_lpspi.c)
zephyr_library_sources_ifdef(CONFIG_UART_MCUX_LPUART	fsl_lpuart.c)
zephyr_library_sources_ifdef(CONFIG_VIDEO_MCUX_CSI	fsl_csi.c)
zephyr_library_sources_ifdef(CONFIG_WDT_MCUX_IMX_WDOG	fsl_wdog.c)

if(CONFIG_FLASH_MCUX_FLEXSPI_XIP)
  zephyr_code_relocate(fsl_flexspi.c ${CONFIG_FLASH_MCUX_FLEXSPI_XIP_MEM}_TEXT)
endif()

if(NOT CONFIG_ASSERT OR CONFIG_FORCE_NO_ASSERT)
  zephyr_compile_definitions(NDEBUG) # squelch fsl_flexcan.c warning
endif()
