# First-Fit-Algorithm-Simulation-in-Dynamic-Memory-Allocation
Mini Project
class MemoryBlock:
    def __init__(self, size):
        """Initialize a memory block with size and free status."""
        self.size = size
        self.is_allocated = False
        self.process_size = 0  # Size of the process occupying the block

    def allocate(self, process_size):
        """Allocate the block if it's free and large enough."""
        if not self.is_allocated and self.size >= process_size:
            self.is_allocated = True
            self.process_size = process_size
            return True
        return False

    def free(self):
        """Free the block."""
        self.is_allocated = False
        self.process_size = 0

    def __str__(self):
        """Display the memory block status."""
        status = 'Allocated' if self.is_allocated else 'Free'
        return f"Block Size: {self.size}, Status: {status}, Process Size: {self.process_size if self.is_allocated else 'N/A'}"


class FirstFitAllocator:
    def __init__(self, memory_blocks):
        """Initialize with a list of memory blocks."""
        self.memory_blocks = memory_blocks

    def allocate(self, process_size):
        """Allocate memory using the First Fit algorithm."""
        for block in self.memory_blocks:
            if block.allocate(process_size):
                print(f"Process of size {process_size} allocated in block of size {block.size}")
                return True
        print(f"Unable to allocate process of size {process_size}")
        return False

    def free(self, process_size):
        """Free memory blocks that match the process size."""
        for block in self.memory_blocks:
            if block.is_allocated and block.process_size == process_size:
                block.free()
                print(f"Process of size {process_size} has been freed.")
                return True
        print(f"No allocated process found with size {process_size}")
        return False

    def display_memory(self):
        """Display the current state of all memory blocks."""
        print("\nMemory Blocks State:")
        for block in self.memory_blocks:
            print(block)


# Example Usage:
if __name__ == "__main__":
    # Create memory blocks with different sizes
    memory_blocks = [MemoryBlock(100), MemoryBlock(200), MemoryBlock(300), MemoryBlock(400)]

    # Initialize FirstFitAllocator with the created memory blocks
    allocator = FirstFitAllocator(memory_blocks)

    # Display initial memory state
    allocator.display_memory()

    # Allocate memory blocks
    allocator.allocate(150)  # Allocates process of size 150
    allocator.allocate(100)  # Allocates process of size 100
    allocator.allocate(50)   # Allocates process of size 50

    # Display memory state after allocations
    allocator.display_memory()

    # Free some memory
    allocator.free(100)  # Free process of size 100

    # Display memory state after freeing
    allocator.display_memory()
