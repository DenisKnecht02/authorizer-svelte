<script lang="ts">
	import {
		StyledPasswordStrengthWrapper,
		StyledPasswordStrength,
		StyledFlex
	} from '../styledComponents';
	import { validatePassword } from '../utils/common';

	export let value: string;
	export let setDisableButton: Function;

	let componentState: {
		strength: string;
		score: number;
		hasSixChar: boolean;
		hasLowerCase: boolean;
		hasNumericChar: boolean;
		hasSpecialChar: boolean;
		hasUpperCase: boolean;
		maxThirtySixChar: boolean;
		isValid: boolean;
	} = {
		strength: '',
		score: 0,
		hasSixChar: false,
		hasLowerCase: false,
		hasNumericChar: false,
		hasSpecialChar: false,
		hasUpperCase: false,
		maxThirtySixChar: false,
		isValid: false
	};

	$: {
		const validationData = validatePassword(value);
		componentState = validationData;
		if (!validationData.isValid) {
			setDisableButton(true);
		} else {
			setDisableButton(false);
		}
	}
</script>

<div>
	<StyledPasswordStrengthWrapper>
		<StyledFlex alignItems="center" justifyContent="center" wrap="nowrap">
			<StyledPasswordStrength strength={componentState.score > 2 ? 'weak' : 'default'} />
			<StyledPasswordStrength strength={componentState.score > 3 ? 'good' : 'default'} />
			<StyledPasswordStrength strength={componentState.score > 4 ? 'strong' : 'default'} />
			<StyledPasswordStrength strength={componentState.score > 5 ? 'veryStrong' : 'default'} />
			{#if componentState.score}
				<div>{componentState.strength}</div>
			{/if}
		</StyledFlex>
		<p>
			<b>Criteria for a strong password:</b>
		</p>
		<StyledFlex flexDirection="column">
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.hasSixChar} />
				<div style="margin-left: 5px;">At least 6 characters</div>
			</StyledFlex>
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.hasLowerCase} />
				<div style="margin-left: 5px;">At least 1 lowercase letter</div>
			</StyledFlex>
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.hasUpperCase} />
				<div style="margin-left: 5px;">At least 1 uppercase letter</div>
			</StyledFlex>
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.hasNumericChar} />
				<div style="margin-left: 5px;">At least 1 numeric character</div>
			</StyledFlex>
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.hasSpecialChar} />
				<div style="margin-left: 5px;">At least 1 special character</div>
			</StyledFlex>
			<StyledFlex justifyContent="flex-start" alignItems="center" width="100%">
				<input readOnly type="checkbox" checked={componentState.maxThirtySixChar} />
				<div style="margin-left: 5px;">Maximum 36 characters</div>
			</StyledFlex>
		</StyledFlex>
	</StyledPasswordStrengthWrapper>
</div>

<style></style>
