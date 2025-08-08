# funny-3d-game
Game 3D hài hước demo online
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Funny 3D Game</title>
    <style>
        body { margin: 0; overflow: hidden; background: #000; }
        #info {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-family: Arial;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div id="info">🎮 Dùng phím WASD hoặc mũi tên để di chuyển</div>
    <script src="https://cdn.jsdelivr.net/npm/three@0.155.0/build/three.min.js"></script>
    <script>
        // Tạo scene, camera, renderer
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Ánh sáng
        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(5, 10, 7.5);
        scene.add(light);

        // Mặt đất
        const groundGeometry = new THREE.PlaneGeometry(100, 100);
        const groundMaterial = new THREE.MeshStandardMaterial({ color: 0x228B22 });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        scene.add(ground);

        // Nhân vật (hình hộp)
        const playerGeometry = new THREE.BoxGeometry(1, 2, 1);
        const playerMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
        const player = new THREE.Mesh(playerGeometry, playerMaterial);
        player.position.y = 1; // đứng trên mặt đất
        scene.add(player);

        camera.position.set(0, 5, 10);
        camera.lookAt(player.position);

        // Điều khiển
        const keys = {};
        document.addEventListener('keydown', (e) => keys[e.key.toLowerCase()] = true);
        document.addEventListener('keyup', (e) => keys[e.key.toLowerCase()] = false);

        function animate() {
            requestAnimationFrame(animate);

            const speed = 0.1;
            if (keys['w'] || keys['arrowup']) player.position.z -= speed;
            if (keys['s'] || keys['arrowdown']) player.position.z += speed;
            if (keys['a'] || keys['arrowleft']) player.position.x -= speed;
            if (keys['d'] || keys['arrowright']) player.position.x += speed;

            camera.position.x = player.position.x + 5;
            camera.position.z = player.position.z + 10;
            camera.lookAt(player.position);

            renderer.render(scene, camera);
        }
        animate();

        // Responsive
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
