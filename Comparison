You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
image-comparison.tsx
'use client';
import { cn } from '@/lib/utils';
import { useState, createContext, useContext } from 'react';
import {
  motion,
  MotionValue,
  SpringOptions,
  useMotionValue,
  useSpring,
  useTransform,
} from 'framer-motion';

const ImageComparisonContext = createContext<
  | {
      sliderPosition: number;
      setSliderPosition: (pos: number) => void;
      motionSliderPosition: MotionValue<number>;
    }
  | undefined
>(undefined);

type ImageComparisonProps = {
  children: React.ReactNode;
  className?: string;
  enableHover?: boolean;
  springOptions?: SpringOptions;
};

const DEFAULT_SPRING_OPTIONS = {
  bounce: 0,
  duration: 0,
};

function ImageComparison({
  children,
  className,
  enableHover,
  springOptions,
}: ImageComparisonProps) {
  const [isDragging, setIsDragging] = useState(false);
  const motionValue = useMotionValue(50);
  const motionSliderPosition = useSpring(
    motionValue,
    springOptions ?? DEFAULT_SPRING_OPTIONS
  );
  const [sliderPosition, setSliderPosition] = useState(50);

  const handleDrag = (event: React.MouseEvent | React.TouchEvent) => {
    if (!isDragging && !enableHover) return;

    const containerRect = (
      event.currentTarget as HTMLElement
    ).getBoundingClientRect();
    const x =
      'touches' in event
        ? event.touches[0].clientX - containerRect.left
        : (event as React.MouseEvent).clientX - containerRect.left;

    const percentage = Math.min(
      Math.max((x / containerRect.width) * 100, 0),
      100
    );
    motionValue.set(percentage);
    setSliderPosition(percentage);
  };

  return (
    <ImageComparisonContext.Provider
      value={{ sliderPosition, setSliderPosition, motionSliderPosition }}
    >
      <div
        className={cn(
          'relative select-none overflow-hidden',
          enableHover && 'cursor-ew-resize',
          className
        )}
        onMouseMove={handleDrag}
        onMouseDown={() => !enableHover && setIsDragging(true)}
        onMouseUp={() => !enableHover && setIsDragging(false)}
        onMouseLeave={() => !enableHover && setIsDragging(false)}
        onTouchMove={handleDrag}
        onTouchStart={() => !enableHover && setIsDragging(true)}
        onTouchEnd={() => !enableHover && setIsDragging(false)}
      >
        {children}
      </div>
    </ImageComparisonContext.Provider>
  );
}

const ImageComparisonImage = ({
  className,
  alt,
  src,
  position,
}: {
  className?: string;
  alt: string;
  src: string;
  position: 'left' | 'right';
}) => {
  const { motionSliderPosition } = useContext(ImageComparisonContext)!;
  const leftClipPath = useTransform(
    motionSliderPosition,
    (value) => `inset(0 0 0 ${value}%)`
  );
  const rightClipPath = useTransform(
    motionSliderPosition,
    (value) => `inset(0 ${100 - value}% 0 0)`
  );

  return (
    <motion.img
      src={src}
      alt={alt}
      className={cn('absolute inset-0 h-full w-full object-cover', className)}
      style={{
        clipPath: position === 'left' ? leftClipPath : rightClipPath,
      }}
    />
  );
};

const ImageComparisonSlider = ({
  className,
  children,
}: {
  className: string;
  children?: React.ReactNode;
}) => {
  const { motionSliderPosition } = useContext(ImageComparisonContext)!;

  const left = useTransform(motionSliderPosition, (value) => `${value}%`);

  return (
    <motion.div
      className={cn('absolute bottom-0 top-0 w-1 cursor-ew-resize', className)}
      style={{
        left,
      }}
    >
      {children}
    </motion.div>
  );
};

export { ImageComparison, ImageComparisonImage, ImageComparisonSlider };


demo.tsx
"use client"

import * as React from "react"
import {
  ImageComparison,
  ImageComparisonImage,
  ImageComparisonSlider,
} from "@/components/ui/image-comparison"

export function BasicDemo() {
  return (
    <ImageComparison className="aspect-[16/10] w-full rounded-lg border border-zinc-200 dark:border-zinc-800">
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_dark.png"
        alt="Motion Primitives Dark"
        position="left"
      />
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_light.png"
        alt="Motion Primitives Light"
        position="right"
      />
      <ImageComparisonSlider className="bg-white" />
    </ImageComparison>
  )
}

export function HoverDemo() {
  return (
    <ImageComparison
      className="aspect-[16/10] w-full rounded-lg border border-zinc-200 dark:border-zinc-800"
      enableHover
    >
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_dark.png"
        alt="Motion Primitives Dark"
        position="left"
      />
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_light.png"
        alt="Motion Primitives Light"
        position="right"
      />
      <ImageComparisonSlider className="bg-white" />
    </ImageComparison>
  )
}

export function SpringDemo() {
  return (
    <ImageComparison
      className="aspect-[16/10] w-full rounded-lg border border-zinc-200 dark:border-zinc-800"
      enableHover
      springOptions={{
        bounce: 0.3,
      }}
    >
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_dark.png"
        alt="Motion Primitives Dark"
        position="left"
      />
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_light.png"
        alt="Motion Primitives Light"
        position="right"
      />
      <ImageComparisonSlider className="w-0.5 bg-white/30 backdrop-blur-sm" />
    </ImageComparison>
  )
}

export function CustomSliderDemo() {
  return (
    <ImageComparison className="aspect-[16/10] w-full rounded-lg border border-zinc-200 dark:border-zinc-800">
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_dark.png"
        alt="Motion Primitives Dark"
        position="left"
      />
      <ImageComparisonImage
        src="https://motion-primitives.com/mp_light.png"
        alt="Motion Primitives Light"
        position="right"
      />
      <ImageComparisonSlider className="w-2 bg-white/50 backdrop-blur-sm transition-colors hover:bg-white/80">
        <div className="absolute left-1/2 top-1/2 h-8 w-6 -translate-x-1/2 -translate-y-1/2 rounded-[4px] bg-white" />
      </ImageComparisonSlider>
    </ImageComparison>
  )
}

export const demos = [
  {
    name: "Basic",
    description: "Basic image comparison with default settings",
    component: BasicDemo,
  },
  {
    name: "Hover",
    description: "Image comparison with hover interaction enabled",
    component: HoverDemo,
  },
  {
    name: "Spring Animation",
    description: "Image comparison with spring animation effect",
    component: SpringDemo,
  },
  {
    name: "Custom Slider",
    description: "Image comparison with custom slider design",
    component: CustomSliderDemo,
  }
]

```

Install NPM dependencies:
```bash
framer-motion
```

Implementation Guidelines
 1. Analyze the component structure and identify all required dependencies
 2. Review the component's argumens and state
 3. Identify any required context providers or hooks and install them
 4. Questions to Ask
 - What data/props will be passed to this component?
 - Are there any specific state management requirements?
 - Are there any required assets (images, icons, etc.)?
 - What is the expected responsive behavior?
 - What is the best place to use this component in the app?

Steps to integrate
 0. Copy paste all the code above in the correct directories
 1. Install external dependencies
 2. Fill image assets with Unsplash stock images you know exist
 3. Use lucide-react icons for svgs or logos if component requires them
