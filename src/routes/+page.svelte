<script lang="ts">
	import { onMount } from 'svelte';

	let track;
	let mouseDownAt = 0;
	let prevPercentage = 0;
	const handleOnDown = (e) => {
		mouseDownAt = e.clientX;
	};
	const handleOnUp = () => {
		mouseDownAt = 0;
		prevPercentage = track.dataset.percentage;
	};
	const handleOnMove = (e) => {
		if (mouseDownAt === 0) return;
		const mouseDelta = parseFloat(mouseDownAt) - e.clientX;
		const maxDelta = window.innerWidth / 2;
		const percentage = (mouseDelta / maxDelta) * -100;
		const nextPercentageUnconstrained = parseFloat(prevPercentage) + percentage;
		const nextPercentage = Math.max(
			Math.min(nextPercentageUnconstrained, 0),
			-100
		);
		const _nxt = Math.max(Math.min(nextPercentageUnconstrained, 0), -100);
		track.dataset.percentage = _nxt;
		track.animate(
			{
				transform: `translate(${_nxt}%, -50%)`,
			},
			{ duration: 1200, fill: 'forwards' }
		);
		const images = track.getElementsByClassName('image');
		for (const image of images) {
			image.animate(
				{
					objectPosition: `${100 + nextPercentage}% center`,
				},
				{ duration: 1200, fill: 'forwards' }
			);
		}
	};
	onMount(() => {
		track = document.getElementById('image-track');
		window.addEventListener('mousedown', handleOnDown);
		window.addEventListener('touchstart', (e) => handleOnDown(e.touches[0]));
		window.addEventListener('mouseup', handleOnUp);
		window.addEventListener('touchend', (e) => handleOnUp(e.touches[0]));
		window.addEventListener('mousemove', handleOnMove);
		window.addEventListener('touchmove', (e) => handleOnMove(e.touches[0]));
		return () => {
			window.removeEventListener('mousedown', handleOnDown);
			window.removeEventListener('touchstart', (e) =>
				handleOnDown(e.touches[0])
			);
			window.removeEventListener('mouseup', handleOnUp);
			window.removeEventListener('touchend', (e) => handleOnUp(e.touches[0]));
			window.removeEventListener('mousemove', handleOnMove);
			window.removeEventListener('touchmove', (e) =>
				handleOnMove(e.touches[0])
			);
		};
	});
</script>

<div class="wrapper">
	<div id="image-track" data-mouse-down-at="0" data-prev-percentage="0">
		<img
			class="image"
			src="https://dxbhsrqyrr690.cloudfront.net/sidearm.nextgen.sites/nsusharks.com/images/2022/12/3/Image__20_.jpeg"
			draggable="false"
		/>
		<img
			class="image"
			src="https://images.unsplash.com/photo-1610194352361-4c81a6a8967e?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1674&q=80"
			draggable="false"
		/>
		<img
			class="image"
			src="https://images.unsplash.com/photo-1618202133208-2907bebba9e1?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
			draggable="false"
		/>
		<img
			class="image"
			src="https://images.unsplash.com/photo-1495805442109-bf1cf975750b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
			draggable="false"
		/>
		<img
			class="image"
			src="https://images.unsplash.com/photo-1548021682-1720ed403a5b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1770&q=80"
			draggable="false"
		/>
		<img
			class="image"
			src="https://asset.dr.dk/imagescaler/?protocol=https&server=www.dr.dk&file=%2Fimages%2Fcrop%2F2021%2F05%2F12%2F1620812428_landshold_under_kurv_0.jpg&quality=70"
			draggable="false"
		/>
		<img
			class="image"
			src="https://dxbhsrqyrr690.cloudfront.net/sidearm.nextgen.sites/gatorzone.com/images/2021/11/5/Alberte_1.jpg"
			draggable="false"
		/>
		<img
			class="image"
			src="https://dxbhsrqyrr690.cloudfront.net/sidearm.nextgen.sites/nsusharks.com/images/2022/12/3/Image__20_.jpeg"
			draggable="false"
		/>
	</div>
</div>

<style>
	.wrapper {
		height: 400px;
		width: 100vw;
		background-color: black;
		margin: 0rem;
		overflow: hidden;
		position: relative;
	}
	#image-track {
		display: flex;
		gap: 4vmin;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(0%, -50%);
		user-select: none; /* -- Prevent image highlighting -- */
	}
	#image-track > .image {
		width: 240px;
		height: 350px;
		object-fit: cover;
		object-position: 100% center;
	}
	/* -- YouTube Link Styles -- */
	body.menu-toggled > .meta-link > span {
		color: rgb(30, 30, 30);
	}
	#source-link {
		bottom: 60px;
	}
	#source-link > i {
		color: rgb(94, 106, 210);
	}
	#yt-link > i {
		color: rgb(239, 83, 80);
	}
	.meta-link {
		align-items: center;
		backdrop-filter: blur(3px);
		background-color: rgba(255, 255, 255, 0.05);
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 6px;
		bottom: 10px;
		box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.1);
		cursor: pointer;
		display: inline-flex;
		gap: 5px;
		left: 10px;
		padding: 10px 20px;
		position: fixed;
		text-decoration: none;
		transition: background-color 400ms, border-color 400ms;
		z-index: 10000;
	}
	.meta-link:hover {
		background-color: rgba(255, 255, 255, 0.1);
		border: 1px solid rgba(255, 255, 255, 0.2);
	}
	.meta-link > i,
	.meta-link > span {
		height: 20px;
		line-height: 20px;
	}
	.meta-link > span {
		color: white;
		font-family: 'Rubik', sans-serif;
		font-weight: 500;
	}
</style>
