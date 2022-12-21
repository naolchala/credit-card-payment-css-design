<script lang="ts">
	let flipped = false;
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
						<span class="group">####</span>
						<span class="group">####</span>
						<span class="group">####</span>
						<span class="group">####</span>
					</div>
					<div class="name-date">
						<div class="name-field">
							<span class="name-label">Card Holder</span>
							<span class="name">Full Name</span>
						</div>
						<div class="date-field">
							<span class="date-label">Expire</span>
							<div class="date-value">
								<span class="month">23</span>
								<span class="slash">/</span>
								<span class="year">12</span>
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
							<div class="four-digits">3456</div>
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
				<input pattern="(\d\d\d\d \d\d\d\d \d\d\d\d)" id="cardNumber" />
			</div>
			<div class="text-field">
				<label for="cardHolder">Card Holder</label>
				<input type="text" name="" id="cardHolder" />
			</div>
			<div class="expiration-container">
				<div class="text-field">
					<label for="exp-date">Expiration Date</label>
					<select name="" placeholder="Month" id="exp-date">
						{#each [...Array(31)] as date, index (index)}
							<option value={index + 1}>{index + 1}</option>
						{/each}
					</select>
				</div>
				<div class="text-field">
					<label for="">&nbsp;</label>
					<select placeholder="Year" id="exp-date">
						{#each [...Array(12)] as date, index (index)}
							<option>{index + 1}</option>
						{/each}
					</select>
				</div>

				<div class="text-field">
					<label for="cw">CW</label>
					<input type="text" maxlength="4" name="" id="cw" />
				</div>
			</div>
			<button on:click={() => (flipped = !flipped)}>submit</button>
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
							width: 40px;
						}
					}

					.credit-card--number {
						flex: 1;
						display: flex;
						font-size: 23px;
						font-weight: bold;
						justify-content: space-evenly;
						align-items: center;
					}
					.name-date {
						display: flex;

						.name-field {
							flex: 1;
							display: flex;
							flex-direction: column;

							.name-label {
								font-size: 14px;
								margin-bottom: 3px;
							}

							.name {
								font-weight: bold;
								text-transform: uppercase;
								font-size: 16px;
							}
						}
						.date-field {
							display: flex;
							flex-direction: column;

							.date-label {
								font-size: 14px;
								margin-bottom: 3px;
							}

							.date-value {
								font-weight: bold;
								text-transform: uppercase;
								font-size: 16px;
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
							font-size: 14px;
							margin-bottom: 5px;
						}

						.cw-value {
							border-radius: 5px;
							background-color: white;
							padding: 8px;
							width: 100%;
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
						width: 50px;
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
