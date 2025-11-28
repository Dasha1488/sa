<template>
  <div class="calculator">
    <div class="calculator__header">
      <h2>Калькулятор подсетей</h2>
    </div>
    
    <div class="calculator__inputs">
      <div class="input-group">
        <label for="ip-input">IP адрес:</label>
        <input
          id="ip-input"
          v-model="ipAddress"
          type="text"
          placeholder="192.168.1.150"
          class="input"
          :class="{ 'input--error': !isIpValid(ipAddress) && ipAddress !== '' }"
          @keyup.enter="calculate"
        >
        <div v-if="!isIpValid(ipAddress) && ipAddress !== ''" class="error-message">
          Неверный формат IP адреса
        </div>
      </div>
      
      <div class="input-group">
        <label for="mask-select">Сетевая маска:</label>
        <select
          id="mask-select"
          v-model="selectedMask"
          class="select"
        >
          <option
            v-for="option in MASK_OPTIONS"
            :key="option.value"
            :value="getMaskValue(option.text)"
          >
            {{ option.text }}
          </option>
        </select>
      </div>
      
      <button
        class="button"
        :disabled="!isCalculateEnabled"
        @click="calculate"
      >
        Рассчитать
      </button>
    </div>
    
    <div v-if="showResults" class="calculator__results">
      <h3>Результаты расчета:</h3>
      <div class="result-item">
        <strong>IP адрес:</strong> {{ ipAddress }}
      </div>
      <div class="result-item">
        <strong>Маска сети:</strong> {{ selectedMask }}
      </div>
      <div class="result-item">
        <strong>Адрес сети:</strong> {{ networkAddress }}
      </div>
      <div class="result-item">
        <strong>Количество адресов:</strong> {{ addressesCount }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { MASK_OPTIONS } from '@/constants/maskOptions'
import { isIpValid, getNetworkAddress, getAddressesCount } from '@/utils/ipCalculator'

const ipAddress = ref('')
const selectedMask = ref('255.255.255.0')
const networkAddress = ref('')
const addressesCount = ref(0)
const showResults = ref(false)

const isCalculateEnabled = computed(() => {
  return isIpValid(ipAddress.value) && ipAddress.value !== ''
})

function getMaskValue(maskText: string): string {
  return maskText.split('/')[1]
}

function calculate() {
  if (!isCalculateEnabled.value) return
  
  networkAddress.value = getNetworkAddress(ipAddress.value, selectedMask.value)
  addressesCount.value = getAddressesCount(selectedMask.value)
  showResults.value = true
}
</script>

<style scoped>
.calculator {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.calculator__header {
  text-align: center;
  margin-bottom: 24px;
}

.calculator__header h2 {
  color: #333;
  margin: 0;
}

.calculator__inputs {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

label {
  font-weight: 500;
  color: #555;
}

.input {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.2s;
}

.input:focus {
  outline: none;
  border-color: #007bff;
}

.input--error {
  border-color: #dc3545;
}

.error-message {
  color: #dc3545;
  font-size: 12px;
}

.select {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  background-color: white;
  cursor: pointer;
}

.button {
  padding: 10px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.button:hover:not(:disabled) {
  background-color: #0056b3;
}

.button:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.calculator__results {
  margin-top: 24px;
  padding: 16px;
  background-color: #f8f9fa;
  border-radius: 4px;
  border: 1px solid #e9ecef;
}

.calculator__results h3 {
  margin: 0 0 12px 0;
  color: #333;
}

.result-item {
  margin-bottom: 8px;
  padding: 4px 0;
}

.result-item strong {
  color: #555;
}
</style>