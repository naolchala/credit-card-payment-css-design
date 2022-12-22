<script lang="ts">
	import { fly, slide, blur, scale, fade } from "svelte/transition";
	let flipped = false;
	let holderName = "";
	$: nameDisplay = holderName == "" ? "Full name" : holderName;
	let year = 2022;
	let month = 1;
	let creditNumber;
	$: numArray = `${creditNumber || ""}`.split("");
	$: creditDisplay = "#### #### #### ####".split(" ").map((group, gI) => [
		...group.split("").map((g, cI) => {
			let index = gI * 4 + cI;
			let value = numArray[index];
			if (!value) {
				value = "#";
			}
			return value;
		}),
	]);

	let cw: number;
	$: cwDisplay = `${cw || ""}`.split("");
	$: yearDisplay = ("" + year).substring(2, 4);
	$: monthDisplay = month < 10 ? "0" + month : month;
	$: nameList = nameDisplay.split("");
</script>

<main>
	<div class="main-container">
		<div class="credit-card" class:flipped>
			<div class="front-card">
				<div class="front-container">
					<div class="logos">
						<img src="/chip.png" alt="" class="card" />
						<img src="/visa.png" alt="" class="visa" />
					</div>
					<div class="credit-card--number">
						{#each creditDisplay as creditGroup, parentIndex (parentIndex)}
							<span class="group">
								{#each creditGroup as creditNumber, childIndex (childIndex)}
									<span class="credit-number-container">
										<span class="credit-num" in:fade>
											{creditNumber}
										</span>
									</span>
								{/each}
							</span>
						{/each}
					</div>
					<div class="name-date">
						<div class="name-field">
							<span class="name-label">Card Holder</span>
							<span class="name">
								{#each nameList as spell, index (index)}
									<span class="spell-container">
										<span
											in:fly={{ y: 20 }}
											out:fly={{ y: -20 }}
										>
											{#if spell == ""}
												" " &nbsp;
											{:else}
												{spell}
											{/if}
										</span>
									</span>
								{/each}
							</span>
						</div>
						<div class="date-field">
							<span class="date-label">Expire</span>
							<div class="date-value">
								<span class="month-container">
									{#key monthDisplay}
										<span
											class="month"
											in:fly={{ y: 10 }}
											out:fly={{ y: -10 }}
										>
											{monthDisplay}
										</span>
									{/key}
								</span>
								<span class="month-container slash">
									<span class="month" in:scale out:scale>
										/
									</span>
								</span>
								<span class="month-container">
									{#key yearDisplay}
										<span class="month" in:scale out:scale>
											{yearDisplay}
										</span>
									{/key}
								</span>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="back-card">
				<div class="back-container">
					<div class="black-box" />
					<div class="cw-field">
						<span class="cw-label">CW</span>
						<div class="cw-value">
							<div class="cw-holder" />
							<div class="four-digits">
								{#each cwDisplay as cwSpell, index (index)}
									<span in:scale out:scale class="dot" />
								{/each}
							</div>
						</div>
					</div>
					<div class="visa-logo">
						<img src="/visa.png" alt="" />
					</div>
				</div>
			</div>
		</div>
		<div class="form">
			<div class="text-field">
				<label for="cardNumber">Card Number</label>
				<input
					bind:value={creditNumber}
					type="number"
					maxlength="16"
					pattern="(\d\d\d\d \d\d\d\d \d\d\d\d)"
					id="cardNumber"
				/>
			</div>
			<div class="text-field">
				<label for="cardHolder">Card Holder</label>
				<input
					bind:value={holderName}
					type="text"
					name=""
					id="cardHolder"
				/>
			</div>
			<div class="expiration-container">
				<div class="text-field">
					<label for="exp-date">Expiration Date</label>
					<select
						bind:value={month}
						name=""
						placeholder="Month"
						id="exp-date"
					>
						{#each [...Array(12)] as month, index (index)}
							<option value={index + 1}>{index + 1}</option>
						{/each}
					</select>
				</div>
				<div class="text-field">
					<label for="">&nbsp;</label>
					<select bind:value={year} placeholder="Year" id="exp-date">
						{#each [...Array(20)] as year, index (index)}
							<option>{index + 2010}</option>
						{/each}
					</select>
				</div>

				<div class="text-field">
					<label for="cw">CW</label>
					<input
						on:focus={() => (flipped = true)}
						on:blur={() => (flipped = false)}
						bind:value={cw}
						type="text"
						maxlength="4"
						name=""
						id="cw"
					/>
				</div>
			</div>
			<button>submit</button>
		</div>
	</div>
</main>

<style lang="scss">
	main {
		width: 100%;
		height: 100vh;

		display: flex;
		flex-direction: column;
		align-items: center;
		overflow: auto;
	}
	.main-container {
		width: clamp(400px, 40%, 500px);
		display: flex;
		flex-direction: column;
		align-items: center;
		perspective: 1000px;
		perspective-origin: top;

		.credit-card {
			position: relative;
			width: 80%;
			top: 100px;
			height: 220px;
			border-radius: 10px;
			transition: all 500ms ease-in-out;
			transform-style: preserve-3d;
			transform-origin: top;
			backface-visibility: hidden;

			&.flipped {
				transform: rotateY(180deg) perspective(1000px);
			}

			.front-card,
			.back-card {
				backface-visibility: hidden;
				transition: all 300ms ease-in-out;
			}

			.front-card {
				position: absolute;
				top: 0;
				bottom: 0;
				right: 0;
				left: 0;

				background: #000a url("/card-bg.jpg");
				background-blend-mode: overlay;
				background-size: cover;
				border-radius: 10px;
				box-shadow: rgba(255, 255, 255, 0.1) 0px 1px 1px 0px inset,
					rgba(50, 50, 93, 0.25) 0px 50px 100px -20px,
					rgba(0, 0, 0, 0.3) 0px 30px 60px -30px;
				transform: rotateY(0deg) perspective(1000px);

				.front-container {
					display: flex;
					flex-direction: column;
					align-content: center;
					justify-content: space-around;
					padding: 20px;
					height: 100%;
					color: white;

					.logos {
						display: flex;
						justify-content: space-between;

						img {
							width: 50px;
						}
					}

					.credit-card--number {
						flex: 1;
						display: flex;
						font-size: 23px;
						font-weight: bold;
						justify-content: space-evenly;
						align-items: center;

						.group {
							display: flex;
							.credit-number-container {
								position: relative;
								width: 17px;
								height: 30px;

								.credit-num {
									position: absolute;
									top: 0;
									bottom: 0;
									right: 0;
									left: 0;

									display: flex;
									align-items: center;
									justify-content: center;
								}
							}
						}
					}
					.name-date {
						display: flex;

						.name-field {
							flex: 1;
							display: flex;
							flex-direction: column;

							.name-label {
								font-size: 11px;
								margin-bottom: 3px;
							}

							.name {
								font-weight: bold;
								text-transform: uppercase;
								font-size: 14px;
								display: flex;

								.spell-container {
									position: relative;
									width: 13px;
									height: 20px;
									overflow: hidden;

									span {
										position: absolute;
										left: 0;
										right: 0;
										bottom: 0;
										top: 0;
										display: flex;
										align-items: center;
										justify-content: center;
									}
								}

								span {
									white-space: pre;
								}
							}
						}
						.date-field {
							display: flex;
							flex-direction: column;

							.date-label {
								font-size: 11px;
								margin-bottom: 3px;
							}

							.date-value {
								display: flex;
								justify-content: space-around;
								font-weight: bold;
								text-transform: uppercase;

								.month-container {
									position: relative;
									width: 25px;
									height: 20px;
									overflow: hidden;

									&.slash {
										width: 10px;
									}

									.month {
										position: absolute;
										left: 0;
										right: 0;
										bottom: 0;
										top: 0;
										display: flex;
										align-items: center;
										justify-content: center;
									}
								}
							}
						}
					}
				}
			}
			.back-card {
				height: 100%;
				width: 100%;
				position: absolute;
				top: 0;
				bottom: 0;
				right: 0;
				left: 0;

				box-shadow: rgba(255, 255, 255, 0.1) 0px 1px 1px 0px inset,
					rgba(50, 50, 93, 0.25) 0px 50px 100px -20px,
					rgba(0, 0, 0, 0.3) 0px 30px 60px -30px;
				transform: rotateY(180deg);

				background: #0006 url("/card-bg.jpg");
				background-blend-mode: overlay;
				background-size: cover;
				border-radius: 10px;
				color: white;

				.back-container {
					height: 100%;
					display: flex;
					flex-direction: column;
					justify-content: space-evenly;

					.black-box {
						display: block;
						height: 40px;
						background-color: #000c;
						width: 100%;
					}

					.cw-field {
						margin: 5px;
						display: flex;
						flex-direction: column;

						.cw-label {
							align-self: flex-end;
							font-size: 12px;
							margin-bottom: 5px;
							font-weight: bold;
						}

						.cw-value {
							border-radius: 5px;
							background-color: white;
							padding: 8px;
							width: 100%;
							display: flex;
							align-items: center;
							height: 30px;

							.cw-holder {
								flex: 1;
							}

							.four-digits {
								display: flex;
								.dot {
									width: 5px;
									height: 5px;
									display: block;
									background-color: black;
									border-radius: 100%;
									margin: 0 3px;
								}
							}
						}
					}

					.visa-logo {
						padding: 0 20px;
						display: flex;
						flex-direction: column;
						width: 100%;

						img {
							align-self: flex-end;
							height: 60px;
						}
					}
				}
			}
		}

		.form {
			width: 100%;
			background-color: white;
			padding: 28px;
			padding-top: 130px;
			border-radius: 8px;
			box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;

			.text-field {
				display: flex;
				flex-direction: column;
				margin: 20px 0;

				* {
					transition: all 300ms ease-in-out;
				}

				label {
					color: #777;
					font-size: 12px;
					margin-bottom: 5px;
				}

				input,
				select {
					outline: none;
					padding: 8px;
					border: 1px solid #bfbfbf;
					border-radius: 4px;
					font-size: 12px;
					color: rgb(67, 67, 67);
					background-color: white;

					&:focus {
						border-color: #2196f3;
						box-shadow: rgba(17, 17, 26, 0.05) 0px 4px 16px,
							rgba(17, 17, 26, 0.05) 0px 8px 32px;
					}

					&#cw {
						width: 100px;
					}
				}
			}

			.expiration-container {
				display: flex;
				gap: 10px;

				.text-field {
					flex: 1;
				}

				.text-field:has(input#cw) {
					margin-left: 30px;
				}
			}

			button {
				margin-top: 30px;
				width: 100%;
				padding: 10px;
				border-radius: 4px;
				font-size: 13px;
				font-weight: 500;
				color: white;
				background-color: #1565c0;
				border: none;
				outline: none;
				text-transform: capitalize;
				box-shadow: rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
					rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
				transition: all 300ms ease;

				&:hover {
					background-color: #0d47a1;
				}
			}
		}
	}
</style>
