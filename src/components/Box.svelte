<script lang="ts">
	let shadowObj = {
		xs: "0 0 0 1px rgba(0, 0, 0, 0.05)",
		sm: "0 1px 2px 0 rgba(0, 0, 0, 0.05)",
		base: "0 1px 3px 0 rgba(0, 0, 0, 0.1),0 1px 2px 0 rgba(0, 0, 0, 0.06)",
		md: "0 4px 6px -1px rgba(0, 0, 0, 0.1),0 2px 4px -1px rgba(0, 0, 0, 0.06)",
		lg: "0 10px 15px -3px rgba(0, 0, 0, 0.1),0 4px 6px -2px rgba(0, 0, 0, 0.05)",
		xl: "0 20px 25px -5px rgba(0, 0, 0, 0.1),0 10px 10px -5px rgba(0, 0, 0, 0.04)",
		"2xl": "0 25px 50px -12px rgba(0, 0, 0, 0.25)",
		outline: "0 0 0 3px rgba(66, 153, 225, 0.6)",
		inner: "inset 0 2px 4px 0 rgba(0,0,0,0.06)",
		none: "none",
		darkLg: "rgba(0, 0, 0, 0.1) 0px 0px 0px 1px,rgba(0, 0, 0, 0.2) 0px 5px 10px,rgba(0, 0, 0, 0.4) 0px 15px 40px",
	};

	type ShadowType = keyof typeof shadowObj;

	type nil = undefined | null;
	type PropertyType<T> = T | T[];

	export let style: string = "";
	export let otherProps: Record<string, string> = {};

	export let b: PropertyType<string | undefined | boolean> = undefined;
	export let borderColor: PropertyType<string | undefined> = undefined;
	export let borderWidth: PropertyType<string | undefined> = undefined;

	export let h: PropertyType<string | undefined> = undefined;
	export let w: PropertyType<string | undefined> = undefined;

	export let p: PropertyType<string | undefined> = undefined;
	export let pt: PropertyType<string | undefined> = undefined;
	export let pb: PropertyType<string | undefined> = undefined;
	export let pl: PropertyType<string | undefined> = undefined;
	export let pr: PropertyType<string | undefined> = undefined;
	export let px: PropertyType<string | undefined> = undefined;
	export let py: PropertyType<string | undefined> = undefined;

	export let m: PropertyType<string | undefined> = undefined;
	export let mt: PropertyType<string | undefined> = undefined;
	export let mb: PropertyType<string | undefined> = undefined;
	export let ml: PropertyType<string | undefined> = undefined;
	export let mr: PropertyType<string | undefined> = undefined;
	export let mx: PropertyType<string | undefined> = undefined;
	export let my: PropertyType<string | undefined> = undefined;

	export let fs: PropertyType<string | undefined> = undefined;
	export let fc: PropertyType<string | undefined> = undefined;
	export let fw: PropertyType<string | undefined> = undefined;
	export let bg: PropertyType<string | undefined> = undefined;

	export let dir: PropertyType<"row" | "column" | undefined> = "row";
	export let mainAxis: PropertyType<"baseline" | "center" | "end" | "start" | "space-around" | "space-between" | "space-evenly" | "stretch"> =
		undefined;
	export let crossAxis: PropertyType<"baseline" | "center" | "end" | "start" | "space-around" | "space-between" | "space-evenly" | "stretch"> =
		undefined;
	export let center: PropertyType<boolean | undefined> = undefined;

	export let stretch: PropertyType<boolean | undefined | number> = undefined;
	export let shrink: PropertyType<boolean | undefined | number> = undefined;

	export let shadow: PropertyType<undefined | ShadowType> = undefined;

	let screenW: number;

	function calculateShadows(shadow: ShadowType) {
		return shadowObj[shadow] ?? "unset";
	}

	const small = 400;
	const mid = 500;
	const lg = 992;
	const xl = 1280;
	const xxl = 1536;

	type PropertyTypeValue<T> = T extends PropertyType<infer U> ? U : never;

	function valueCalcualtor<T extends PropertyType<any>>(value: T, screenW: number): PropertyTypeValue<T> {
		if (!(value instanceof Array)) {
			return value as PropertyTypeValue<T>;
		}

		if (screenW <= small) return value[0];
		if (screenW <= mid) return value[1] ?? value[0];
		if (screenW <= lg) return value[2] ?? value[1] ?? value[0];
		if (screenW <= xl) return value[3] ?? value[2] ?? value[1] ?? value[0];
		if (screenW <= xxl) return value[4] ?? value[3] ?? value[2] ?? value[1] ?? value[0];
		return value[5] ?? value[4] ?? value[3] ?? value[2] ?? value[1] ?? value[0];
	}
</script>

<svelte:window bind:innerWidth={screenW} />

<div
	{...otherProps}
	style="
	--b:  {valueCalcualtor(b ? `solid` : undefined, screenW)};
	--borderColor: {valueCalcualtor(borderColor ?? (b || 'black'), screenW)};
	--borderWidth: {valueCalcualtor(borderWidth ?? (b || '1px'), screenW)};
	--h: {valueCalcualtor(h, screenW)};
	--w: {valueCalcualtor(w, screenW)};
	--p: {valueCalcualtor(p, screenW)};
	--pt: {valueCalcualtor(pt || py || p, screenW)};
	--pb: {valueCalcualtor(pb || py || p, screenW)};
	--pl: {valueCalcualtor(pl || px || p, screenW)};
	--pr: {valueCalcualtor(pr || px || p, screenW)};
	--mt: {valueCalcualtor(mt || my || m, screenW)};
	--mb: {valueCalcualtor(mb || my || m, screenW)};
	--ml: {valueCalcualtor(ml || mx || m, screenW)};
	--mr: {valueCalcualtor(mr || mx || m, screenW)};
	--dir: {valueCalcualtor(dir, screenW)};
	--mainAxis: {valueCalcualtor(center ? 'center' : mainAxis, screenW)};
	--crossAxis: {valueCalcualtor(center ? 'center' : crossAxis, screenW)};
	--stretch: {valueCalcualtor(stretch === true ? 1 : stretch, screenW)};
	--shrink: {valueCalcualtor(shrink === true ? 1 : shrink, screenW)};
	--fw: {valueCalcualtor(fw, screenW)};
	--fs: {valueCalcualtor(fs, screenW)};
	--fc: {valueCalcualtor(fc, screenW)};
	--bg: {valueCalcualtor(bg, screenW)};
	--shadow: {calculateShadows(valueCalcualtor(shadow, screenW))};
	{style}
"
>
	<slot />
</div>

<style>
	div {
		box-sizing: border-box;
		display: flex;
		border-color: var(--borderColor);
		border-width: var(--borderWidth);
		border-style: var(--b);
		background-color: var(--bg);
		font-size: var(--fs);
		color: var(--fc);
		font-weight: var(--fw);
		height: var(--h);
		width: var(--w);
		padding-top: var(--pt);
		padding-bottom: var(--pb);
		padding-left: var(--pl);
		padding-right: var(--pr);
		margin-top: var(--mt);
		margin-bottom: var(--mb);
		margin-left: var(--ml);
		margin-right: var(--mr);
		flex-direction: var(--dir);
		justify-content: var(--mainAxis);
		align-items: var(--crossAxis);
		flex-grow: var(--stretch);
		flex-shrink: var(--shrink);
		box-shadow: var(--shadow);
	}
</style>
