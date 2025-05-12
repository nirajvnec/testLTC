import { Utilities } from './Utilities';
import { AxiosError } from 'axios';

const simulateBlankError = () => {
  const fakeError = {
    message: '',
    config: {},
    isAxiosError: true
  } as AxiosError;

  Utilities.handleAxiosError(fakeError, 'simulateBlankError', (msg) => {
    // This should NOT be called if msg is blank
    console.log('Overlay should show this:', msg);
  });
};